---
description: >-
  Short definitions of the vocabulary that shows up throughout 10102 Computing
  Legacy. Skim this first; come back when a term doesn't click.
---

# Concepts

## Legacy type

How your assets pass to your beneficiaries. Two shapes:

- **Transfer legacy**: distributes specific assets to specific addresses when activated, proportional to allocations you set. Created from your connected EOA wallet.
- **Multisig legacy**: hands over your existing Safe by adding your beneficiaries as co-signers on activation. The Safe itself (and everything it holds, stakes, or governs) is the legacy.

The app calls a Transfer legacy made from your wallet a **wallet legacy**. "Your legacy" is the whole, everything you leave together; one contract is a "legacy contract".

## Beneficiaries and heirs

The people you name. The create form says **beneficiaries**; the pages of the new version of the wallet legacy say **heirs**. They are the same people: the first ones named, each with a share.

## The new version of the wallet legacy

Every wallet legacy created on Ethereum since 2026-11-13 20:42 UTC. It adds [NFTs](./legacy/nfts-in-a-legacy.md), [backup heirs](./premium-features/manage-contingent-beneficiaries.md#backup-heirs-for-each-share) and an [executor](./premium-features/executor.md). Older wallet legacies keep their code and their screens, and their page offers a guided [move to the new version](./legacy/move-to-the-new-version.md). It is live without an independent audit; see [Legacy](./legacy/README.md#honest-limits-of-the-new-version).

## Activation trigger

The condition that makes a legacy claimable. Today there's one trigger: **inactivity**, the time since your last activity the legacy can see (what counts differs by wallet type; see [Heartbeat](#heartbeat)). Measured in whole months. When the window elapses, any beneficiary can activate. On a wallet legacy of the new version, an [executor](./premium-features/executor.md) you named can confirm that you have died once you have been silent for half your check-in period, which brings that date forward to 30 days after the confirmation if that is sooner; any check-in from you cancels it.

## Claim fee

The fee taken from what your heirs receive when they claim tokens or ETH from a legacy: 0.25% today. It is fixed into each legacy when it is created and shown before you confirm; a new legacy never accepts more than 0.5%. Legacies created before 2026-09-29 carry none, and NFTs carry none. The terms call it the delivery fee. See [Create a Legacy Contract](./legacy/create-a-legacy-contract.md).

## Heartbeat

Any action that proves you're still around and resets the activation timer. What counts depends on your wallet type:

- **Safe legacies**: every Safe transaction counts automatically (the Safe Guard records it), so normal usage keeps the legacy fresh.
- **EOA legacies**: checking in resets the countdown, and so does any change you make to the legacy. Sending, swapping or signing elsewhere does not, so checking in is up to you. With Premium, [automatic renewal](#auto-renew-eoa-legacies) can count that activity too.

If you don't have another qualifying action to make, **Check in** on the details page resets the timer in a single transaction. With Premium, it is a free signature.

## Auto-renew (EOA legacies)

An optional Premium feature, off by default, that you enable per legacy once it exists. When on, an attestor service watches your wallet's public on-chain activity (its transaction count). If the wallet has sent a transaction since the countdown last restarted, it resets the countdown for you in the last 30 days before your deadline: no click, no gas from you. This works for up to 12 months after your last check-in and while Premium is active; then you check in yourself once and it carries on. Renewals can never make a legacy claimable earlier. See [Automatic Renewal](premium-features/automatic-renewal.md) for the guide, and [EOA Activity & Auto-Renew](../architecture/eoa-activity-auto-renew.md) for the full trust model.

## Creator vs. signer (Safe legacies)

When a Safe creates a legacy, it's the Safe (not an individual) that owns it on-chain. But **the single wallet that submitted the creation transaction** is tagged as the **creator**. On-chain settings (beneficiaries, trigger, allocations) can be changed by any Safe signer at the Safe's threshold. **Off-chain settings** (email reminders, watchers) can only be changed by the creator. This asymmetry is documented inline on the relevant pages.

## Storage token

A substitute for ETH inside the legacy or timelock, because native ETH can't be approved like an ERC-20. When you want to include ETH, you swap it through a built-in route into a supported storage token (WETH, a liquid staking token, etc.) and approve that token instead. You always see the available list in the app.

## CREATE2

The deterministic contract-deployment opcode. We use it so your legacy contract's final address is known _before you sign the deploy transaction_. You can verify it in the app, and beneficiaries can find the contract even if our UI is unavailable. See also [Legacy Claim Card](./legacy/legacy-claim-card.md).

## Safe threshold

The minimum number of Safe signers required to execute a transaction (e.g. 2 of 3). Every on-chain change to a Safe-backed legacy is a Safe transaction and respects your Safe's threshold. You can finalize signatures inside our app or on [app.safe.global](https://app.safe.global); either works.

## Finalizing

When a Safe transaction has collected enough signatures but hasn't executed yet, we call its state _needs finalizing_. Any signer can finalize by paying gas and broadcasting the transaction. Until it's finalized, the change isn't live.

## Watchers

Read-only accounts a Premium user can authorize to view selected legacies under **My Watchlist**. Used for estate lawyers, family oversight, or personal dashboards across wallets. Watchers have one of two visibility levels; see [Manage Authorized Watchers](./premium-features/manage-authorized-watchers.md).

## Backup beneficiary, second backup, backup heirs

Optional Premium layers for when the people you named first do not claim. There are two kinds, and a wallet legacy of the new version can use both:

- **Backup beneficiary** and **second backup** (older pages and the contracts say second line and third line, or contingent beneficiaries): one person for the whole legacy. If your beneficiaries have not claimed a wait after the claim date, the backup beneficiary can claim too, and a claim of theirs sends all it moves to them; the second backup follows after a further wait. Your beneficiaries can still claim after that date, and a claim settles only what it moves.
- **Backup heirs** (new version only): up to two backups for each heir's own share, each after a wait of 30 to 3,650 days. Each share is claimed on its own, so nobody waits for anyone else.

See [Backup Beneficiaries and Backup Heirs](./premium-features/manage-contingent-beneficiaries.md).

## Executor

On a wallet legacy of the new version, with Premium: someone you trust who can confirm to your legacy that you have died, once you have been silent for half your check-in period. Claims then open 30 days later if that is sooner. Nobody verifies the confirmation, and it is a role in the contract only, not an executor of your estate under any law. See [Name an Executor](./premium-features/executor.md).

## NFT list

The collections a legacy can name NFTs from. It is a contract on Ethereum that the app reads; on mainnet it holds three collections since 2026-11-16. Being on the list is no endorsement. See [NFTs in a Legacy](./legacy/nfts-in-a-legacy.md).

## Legacy Claim Card

A printable one-page summary with the minimum information your beneficiaries need to claim via any Ethereum interface, not just our UI. Contains the legacy ID and contract address; intentionally _excludes_ the owner's address for privacy. See [Legacy Claim Card](./legacy/legacy-claim-card.md).

## Timelock

A self-directed time-based lock on your own assets. Three flavors:

- **Timelock**: locked until a date; then you reclaim.
- **Soft timelock**: locked indefinitely; a waiting period starts when _you_ initiate an unlock.
- **Beneficiary timelock**: locked until a date; then a specified recipient claims. Heirloom wraps this type into a designed gift experience with a claim link and free claiming.

See [Timelock](./timelock/README.md).

## Premium subscription

Optional. 99 USD for one year, or 399 USD once for lifetime, paid upfront and on-chain in ETH, USDC, or USDT. It unlocks a backup beneficiary and a second backup, backup heirs and an executor on wallet legacies of the new version, watchers, email reminders, automatic renewal, gas-free check-ins and withdrawals, and wallet alerts for more addresses. Free users retain full access to legacy creation, editing, and claims. See [Premium Features](./premium-features/README.md).
