---
description: >-
  Set up a legacy contract so your assets can pass to your beneficiaries on your
  terms, without a middleman.
---

# Create a Legacy Contract

A legacy contract holds the rules for how, when, and to whom your assets pass. This page walks through creating one end-to-end.

{% hint style="info" %}
**New to the terminology?** See [Concepts](../concepts.md) for short definitions of _legacy type_, _storage token_, _activation trigger_, and _claim card_.
{% endhint %}

## Pick a type

You have two choices at `Create a legacy`:

- **Transfer legacy** — splits your assets between named Ethereum addresses when activated. Created from your connected EOA wallet (MetaMask, Ledger, Trezor, …).
- **Multisig legacy** — hands over control of your existing **Safe** to your beneficiaries by adding them as co-signers once activated.

Still unsure? Start with Transfer legacy — it's the most common case and doesn't require a Safe. You can always create a Multisig legacy later.

{% hint style="success" %}
If you started from a **Quick action** on the home page, we've pre-filled the type and some settings for you. The rest of this page still applies. See [Quick Actions](../quick-actions.md) for the full list.
{% endhint %}

## Create a Transfer legacy

Creating a Transfer legacy is **one transaction**. You configure everything on one screen, confirm once in your wallet, and the contract is live with its asset permissions already in place. Your wallet keeps full custody the whole time — nothing moves until activation.

### Configure

Fill in:

- **Name** — a private label. It's saved on your device only; it is never written to the blockchain, and neither are beneficiary names.
- **Beneficiaries** — up to 10 addresses. If you list only one, we auto-allocate 100%.
- **Allocations** — percentages must sum to 100%.
- **Activation trigger** — inactivity period (time since your last interaction with the legacy) before beneficiaries can claim. Days, not months.
- **Assets to include** — the ERC-20 tokens your beneficiaries will be able to claim. You pick them here, in the same form; there is no separate approval step afterwards.

Tick the terms checkbox. Your acceptance is recorded by the create transaction itself — there's no separate "sign this message" prompt.

{{screenshot: configure-legacy-eoa-step1}}

### Confirm

Click **Deploy**. Two things happen in your wallet:

1. If you included tokens, you sign one **gas-free permission message** covering all of them at once. The permission goes through [Permit2](https://github.com/Uniswap/permit2), the token-permission standard used across the Ethereum ecosystem, and names a single spender: our published, Etherscan-verified vault contract — the same address for every 10102 user. You can check it against the address in [How Legacy Creation Works](../../architecture/create-flow-v2.md).
2. You confirm **one transaction**. It deploys your legacy contract and registers the token permissions together. The contract address is deterministic (`CREATE2`), so the address shown on screen is the address you get.

That's it — the details page opens and your legacy is live.

{% hint style="info" %}
**A possible one-time extra.** If your wallet has never used a particular token with Permit2 before (in any app), that token needs a one-time `approve(Permit2)` transaction first. The app detects this and prompts you. It's once per token per wallet, and it's shared with every other Permit2-based app you use.
{% endhint %}

{% hint style="info" %}
**What does the permission actually allow?** Nothing, until activation. Tokens stay in your wallet and remain fully spendable by you. If the legacy activates, beneficiaries receive their configured percentages of whatever is in your wallet at that time. You can revoke the permission at any point by deleting the legacy or directly in your wallet's Permit2 settings.
{% endhint %}

### Adding ETH

ETH can't be permissioned directly because it isn't an ERC-20. Use the **add ETH** action on the details page to swap it to a **supported storage token** (WETH or a liquid staking token) through a built-in swap route; the permission for the resulting token is set up as part of the same flow.

### Print a Legacy Claim Card (optional but recommended)

The app generates a one-page printable card with the minimum information your beneficiaries need to claim — even if our UI is ever unavailable. See [Legacy Claim Card](./legacy-claim-card.md).

## Safe-based legacies (Multisig / Safe Transfer)

Safe-based flows work differently from the EOA flow above: creation goes through your Safe's own transaction process, so your Safe's co-signers approve it at your normal threshold.

### Create a Multisig legacy

Multisig legacy _is_ your Safe — beneficiaries become co-signers on activation, taking over the wallet and any positions it holds elsewhere (staking, governance, NFTs, etc.).

1. Click **Create a legacy**, choose **Multisig legacy**.
2. Enter your Safe address. We verify it.
3. Configure:
   - Contract name, beneficiaries (addresses), activation trigger.
   - No asset allocation here — beneficiaries take over the Safe itself.
4. Your Safe signers approve the creation transaction (installs our Safe module and guard).

{{screenshot: configure-multisig-check-safe}}

{% hint style="warning" %}
The Safe must still be in active use — Multisig legacy relies on your Safe's last outgoing transaction as a liveness signal. If you don't transact from the Safe at least once per activation window, beneficiaries can activate. Use the heartbeat action on the details page to reset the timer if you don't have another transaction to send.
{% endhint %}

## Premium: Additional contingent beneficiaries

If you subscribe to Premium, you can configure up to two additional contingent layers — fallbacks if a primary beneficiary can't claim (lost keys, deceased, etc.). See [Manage Contingent Beneficiaries](../premium-features/manage-contingent-beneficiaries.md).

## Common questions

**Does the contract hold custody of my assets?** No, for Transfer legacy. Your tokens never leave your wallet before activation. The create transaction only grants a claim permission, and only your own legacy contract can ever use it.

**What happens if I move my included tokens?** Nothing breaks. The permission is a promise, not a lock — beneficiaries can only claim what's actually in your wallet on activation. Spend, stake, or move your tokens as usual.

**Can I add ETH directly to the legacy?** Not for Transfer legacies — ETH must be swapped to a storage token first. This is by design: the legacy contract works through ERC-20 token permissions, which ETH doesn't have.

**Can I include more tokens later?** Yes — use the include action on the details page. Adding a token after creation is a small on-chain transaction.

**Where are the names stored?** The legacy's name and your beneficiaries' names live on your device, not on the blockchain. On-chain there are only addresses, percentages, and the trigger — see [How Legacy Creation Works](../../architecture/create-flow-v2.md).

**Can I change the beneficiaries later?** Yes — see [Edit or Delete a Legacy Contract](./edit-or-delete-a-legacy-contract.md).

## What's next

- [Legacy Contract Details](./legacy-contract-details.md) — what the details view shows, and how to interpret it.
- [Edit or Delete a Legacy Contract](./edit-or-delete-a-legacy-contract.md) — updating allocations, revoking permissions, or tearing everything down.
- [Manage Contingent Beneficiaries](../premium-features/manage-contingent-beneficiaries.md) — Premium fallback layers.
- [How Legacy Creation Works](../../architecture/create-flow-v2.md) — the architecture behind the one-transaction create, for the curious.
