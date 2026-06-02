---
description: >-
  Short definitions of the vocabulary that shows up throughout 10102 Computing
  Legacy. Skim this first; come back when a term doesn't click.
---

# Concepts

## Legacy type

How your assets pass to your beneficiaries. Two shapes:

- **Transfer legacy** — distributes specific assets to specific addresses when activated, proportional to allocations you set. Created from your connected EOA wallet.
- **Multisig legacy** — hands over your existing Safe by adding your beneficiaries as co-signers on activation. The Safe itself (and everything it holds, stakes, or governs) is the legacy.

## Activation trigger

The condition that makes a legacy claimable. Today there's one trigger: **inactivity** — the time since your wallet's (or Safe's) last outgoing transaction. Measured in days. When the window elapses, any beneficiary can activate.

## Heartbeat

Any action that proves you're still around and resets the activation timer. Most on-chain activity from your wallet counts automatically (a send, a swap, an approval, …). If you don't have another transaction to make, the details page has a dedicated heartbeat action that resets the timer in a single transaction.

## Creator vs. signer (Safe legacies)

When a Safe creates a legacy, it's the Safe (not an individual) that owns it on-chain. But **the single wallet that submitted the creation transaction** is tagged as the **creator**. On-chain settings (beneficiaries, trigger, allocations) can be changed by any Safe signer at the Safe's threshold. **Off-chain settings** (email reminders, watchers) can only be changed by the creator. This asymmetry is documented inline on the relevant pages.

## Storage token

A substitute for ETH inside the legacy or timelock, because native ETH can't be approved like an ERC-20. When you want to include ETH, you swap it through a built-in route into a supported storage token (WETH, a liquid staking token, etc.) and approve that token instead. You always see the available list in the app.

## CREATE2

The deterministic contract-deployment opcode. We use it so your legacy contract's final address is known _before you sign the deploy transaction_. You can verify it in the app, and beneficiaries can find the contract even if our UI is unavailable. See also [Legacy Claim Card](./legacy/legacy-claim-card.md).

## Safe threshold

The minimum number of Safe signers required to execute a transaction (e.g. 2 of 3). Every on-chain change to a Safe-backed legacy is a Safe transaction and respects your Safe's threshold. You can finalize signatures inside our app or on [app.safe.global](https://app.safe.global) — either works.

## Finalizing

When a Safe transaction has collected enough signatures but hasn't executed yet, we call its state _needs finalizing_. Any signer can finalize by paying gas and broadcasting the transaction. Until it's finalized, the change isn't live.

## Watchers

Read-only accounts a Premium user can authorize to view selected legacies under **My Watchlist**. Used for estate lawyers, family oversight, or personal dashboards across wallets. Watchers have one of two visibility levels — see [Manage Authorized Watchers](./premium-features/manage-authorized-watchers.md).

## Contingent beneficiaries

Optional Premium layers. If the primary beneficiaries don't claim within a configurable window after activation, a second-line beneficiary becomes eligible. Same thing for a third-line after a further window. Designed to prevent assets from getting stuck when someone loses keys or is unreachable.

## Legacy Claim Card

A printable one-page summary with the minimum information your beneficiaries need to claim via any Ethereum interface — not just our UI. Contains the legacy ID and contract address; intentionally _excludes_ the owner's address for privacy. See [Legacy Claim Card](./legacy/legacy-claim-card.md).

## Timelock

A self-directed time-based lock on your own assets. Three flavors:

- **Timelock** — locked until a date; then you reclaim.
- **Soft timelock** — locked indefinitely; a waiting period starts when _you_ initiate an unlock.
- **Timelocked gift** — locked until a date; then a specified recipient claims.

See [Timelock](./timelock/README.md).

## Premium subscription

Optional, time-bound access model (like ENS registrations). Pay upfront in ETH, USDC, or USDT to unlock contingent beneficiaries, watchers, and email reminders for a chosen period. Free users retain full access to legacy creation, editing, and claims.
