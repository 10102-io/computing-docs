---
description: >-
  Why creating an EOA legacy is a single wallet confirmation: EIP-1167 clones,
  Permit2 permissions to one immutable vault contract, no personal data
  on-chain, and consent recorded by the transaction itself.
---

# How Legacy Creation Works

Creating a Transfer legacy from an EOA wallet used to take three to five wallet interactions: sign an agreement message, send a deploy transaction, then send one `approve` transaction per included token. Today it is **one transaction**. This page explains the architecture that made that possible without weakening anything a user could previously verify — and the trade-offs we accepted along the way.

## The flow at a glance

| | Before | Now (v2) |
|---|---|---|
| Agreement to terms | Separate signed message | Recorded by the create transaction itself |
| Contract deployment | One transaction | Same single transaction |
| Token permissions | One `approve(legacyAddress)` transaction per token | One gas-free Permit2 signature, registered inside the create transaction |
| Permission target | Your freshly-deployed legacy contract (unknown to wallets) | One published, immutable, Etherscan-verified vault — the same address for everyone |
| Names and notes | Stored on-chain | Stored on your device; never on-chain |

The single on-chain call is `TransferEOALegacyRouter.createLegacyV2`. In one transaction it deploys your legacy contract as an EIP-1167 clone at a CREATE2-predicted address, registers your signed Permit2 token permissions, and records your consent to the current terms.

## EIP-1167 clones: pay for state, not code

Every EOA legacy has identical logic and unique state, so deploying the full contract bytecode per user was pure waste. Each legacy is instead a ~45-byte [EIP-1167](https://eips.ethereum.org/EIPS/eip-1167) minimal proxy that delegates to one shared, published implementation — an 80%+ cut in creation gas (roughly 6M → 1.05M on mainnet) — while keeping its own isolated storage (beneficiaries, allocations, trigger, timestamps).

Crucially, **each clone pins its implementation and its vault at creation time**. When we ship a new implementation, it applies to new creations only; your existing contract keeps the exact code it was created with, forever. (An earlier design let existing clones follow a router-held pointer; that is no longer the case.) See the [Upgrade Policy](upgrade-policy.md) for the full picture of what can and cannot change.

## Permit2 + LegacyPullVault: one spender everyone can verify

The hard problem in a one-transaction create is token permissions. Arbitrary ERC-20s don't support gasless approvals natively, so we use [Permit2](https://github.com/Uniswap/permit2), Uniswap's audited, immutable permission contract deployed at the same address on every major chain and shared by much of the ecosystem. At create time you sign one EIP-712 message covering all your included tokens; `createLegacyV2` registers that batch with Permit2 inside the create transaction. Your tokens never move — Permit2 just records that a specific spender may pull them under your signed limits.

**Who is that spender?** Not your legacy contract. At the moment you sign, your legacy contract doesn't exist yet (it's deployed later in the same transaction), and wallets rightly treat a permission naming a code-less address as a drainer fingerprint. A per-user counterfactual address can never be allowlisted, verified, or rendered by name in a wallet.

So the spender is the **LegacyPullVault** — a singleton contract that is:

- **Immutable and admin-free.** No owner, no upgrade path; its behavior was frozen at deployment. On mainnet it lives at [`0x95F0981026C7e804fD6ba8bE738cA7c380C7f978`](https://etherscan.io/address/0x95F0981026C7e804fD6ba8bE738cA7c380C7f978), Etherscan-verified.
- **Codehash-pinned.** The vault only accepts bindings for genuine legacy clones — it checks the EIP-1167 codehash against the exact implementation it was deployed for. A contract with any other code can never bind.
- **First-write-wins.** The create transaction binds your address to your new legacy inside the vault. While that binding is live, it cannot be re-pointed — not even by a router upgrade. Only your own bound legacy can pull your tokens, only through Permit2's per-owner accounting, only within what you signed.
- **Released on delete.** Deleting your legacy clears the binding, after which nothing can pull through the vault for your address.

One stable spender address is what lets wallets show you a consistent, checkable permission request instead of a red warning — and what makes registrations with wallet-security providers and clear-signing descriptors possible at all.

## What the permission actually says

Two properties of the signed Permit2 batch are deliberately broad, and worth stating plainly:

- **The amount is unlimited** (Permit2's maximum). The actual amounts beneficiaries receive are decided by your allocations and your wallet balance at activation — the permission is a ceiling, not a transfer. An itemized amount would go stale the moment you moved funds.
- **The expiration is far in the future** (on the order of decades). A legacy must remain claimable years after you can no longer re-sign anything. A short expiry would silently break the product's core promise.

Both are revocable at any time: delete the legacy, or use Permit2's own `lockdown` / per-token revocation directly from your wallet — no 10102 infrastructure required.

## PII-free events, consent in the transaction

- **Nothing personal goes on-chain.** v2 removed names, nicknames, and notes from legacy storage and events. On-chain there are addresses, percentages, and timing — nothing else. Your legacy's name and your beneficiaries' names are stored on your device (browser local storage), which also means they don't follow you to a new browser unless you re-enter them.
- **Consent is transaction-based.** The terms you accept are published on-chain as a version tag plus a content hash. When you create with the terms checkbox ticked, the create transaction records your consent bound to that exact document hash — wallet-attributable, timestamped, and version-bound, with no separate message-signing ceremony. (The older signed-message path still exists for Safe flows and older frontends.)

## What happens on delete

- **v2 legacies:** the delete transaction releases the vault binding, which disables the Permit2 permissions — no contract can pull through the vault for your address afterwards. For belt-and-suspenders you can also clear the underlying Permit2 entries from your wallet.
- **Pre-v2 legacies** (created with direct ERC-20 approvals): those approvals keep working for the life of the legacy, and the delete flow walks you through revoking each one (`approve(0)` per token).

At claim time, a legacy pulls each token through the most generous rail available to it — a direct ERC-20 allowance, a Permit2 permission naming the legacy itself, or a Permit2 permission through the vault — so every generation of legacy stays claimable without migration.

## The honest trade-offs

- **The vault is shared by every user.** A bug in it would be a bug for everyone. We accepted this because the vault is tiny (~1.5 KiB), immutable, and structurally narrow: it can only forward pulls from an owner's own bound legacy, through Permit2's per-owner signed envelope. The alternative — per-user spenders — is precisely what wallets cannot verify.
- **Permit2 is a trust assumption.** If Permit2 itself had a critical flaw, every token approved to it by any app would be exposed. It is immutable, extensively audited, and carries a large share of ecosystem flow; we accept this on par with using major DeFi infrastructure at all.
- **A narrow admin capability remains.** The role that rotates the clone implementation for *new* creates could, in theory, bind a legacy to an address that has **no** live binding. It can never re-point an existing live binding, and the same role could already change what new creates deploy — so the vault doesn't widen the admin trust surface. That role's upgrade powers now sit behind the public 48-hour queue described in the [Upgrade Policy](upgrade-policy.md).
- **One extra transaction can still appear.** The first time a wallet uses a given token with Permit2 — in any app — a one-time `approve(Permit2)` transaction is required. It amortizes across the whole Permit2 ecosystem, but it's not zero.

## Related pages

- [Legacy Contracts Created with EOAs](legacy-contracts-created-with-eoas.md) — the full EOA legacy lifecycle (heartbeat, editing, activation).
- [Upgrade Policy](upgrade-policy.md) — what can change, who can change it, and how to watch.
- [Create a Legacy Contract](../user-guide/legacy/create-a-legacy-contract.md) — the user-facing walkthrough.
