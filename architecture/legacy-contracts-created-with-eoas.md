---
description: >-
  How Transfer legacies work when the owner is a plain EOA (MetaMask, Ledger,
  Trezor, …) with no Safe involvement: clones, the single-transaction create,
  heartbeat, editing, deletion, activation, the delivery fee, and what legacy
  v3 adds: NFTs, backup beneficiaries, an executor.
---

# Legacy Contracts Created with EOAs

An **externally owned account (EOA)** is an Ethereum account controlled by a single private key, the wallet most people have in their browser or hardware device. EOAs have no on-chain code, which makes them elegant but creates one genuine constraint for a legacy: **there is no built-in way for a smart contract to read an EOA's last outgoing transaction timestamp**. The 10102 EOA flow is shaped around that constraint.

This page describes the pure-EOA **Transfer legacy** path, which the app calls a wallet legacy. Multisig legacies are Safe-only and have their own page, [Legacy Contracts Created with Safe SDK](legacy-contracts-created-with-safe-sdk.md). (A Safe-backed variant of the Transfer legacy existed until May 2026 and was retired without ever having a mainnet legacy.)

## Contract roles

| Contract | Purpose |
|---|---|
| `TransferEOALegacyRouter` | Creates, edits, deletes, and activates EOA-owner Transfer legacies. Holds the address of the shared implementation that new clones are copies of. |
| `LegacyDeployer` | CREATE2 factory. Deploys per-user legacy contracts to a deterministic address. For EOA legacies this deploys an EIP-1167 minimal proxy (~45 bytes of on-chain code) that delegates to the shared implementation. |
| Shared implementation | The actual contract logic. Since 2026-11-13 20:42 UTC new legacies are clones of `TransferEOALegacyV3` ([`0xF9B6241Ab7513181C9Ed4F0920E0B008fB9264e9`](https://etherscan.io/address/0xF9B6241Ab7513181C9Ed4F0920E0B008fB9264e9#code)); older legacies keep the implementation they were created with (table below). |
| Per-legacy clone | A thin EIP-1167 proxy at a CREATE2-predictable address. Stores beneficiaries, allocations, included asset list, activation trigger, and last-activity timestamp in its own storage; executes shared code via `DELEGATECALL`. |
| `LegacyPullVaultV2` | The single Permit2 spender, and the single NFT operator, for every legacy created since 2026-11-07 ([`0xC6640962618f9D729032487EC5DEf226BF6F43E3`](https://etherscan.io/address/0xC6640962618f9D729032487EC5DEf226BF6F43E3#code)). No owner and no upgrade path; its one admin function, reserved to the upgrade timelock, adds an accepted legacy implementation and can remove none. Holds an `owner → legacy` binding; only an owner's own bound legacy can pull their tokens or NFTs. The first `LegacyPullVault` ([`0x95F0981026C7e804fD6ba8bE738cA7c380C7f978`](https://etherscan.io/address/0x95F0981026C7e804fD6ba8bE738cA7c380C7f978#code)) keeps serving the legacies created before. See [How Legacy Creation Works](create-flow-v2.md). |
| `NftCollectionWhiteList` | The list of NFT collections a legacy v3 may name ([`0xF33e887c6BE3bA8Fb583E14164a9e74FD56c1623`](https://etherscan.io/address/0xF33e887c6BE3bA8Fb583E14164a9e74FD56c1623#code)). Read when an owner names an item, never at claim time. |
| `Payment` | Holds the delivery fee rate a new legacy copies at creation, and receives the fee at claim time. |

Routers are upgradeable behind transparent proxies (see the [Upgrade Policy](upgrade-policy.md)). Per-legacy clones are _not_ upgradeable at all: **each clone pins its implementation and its vault at creation time**. Which implementation and which vault *new* clones get is a router setting, and since 2026-11-07 that setting can only be changed through the public 48-hour upgrade timelock. Existing clones keep the exact code they were created with, forever. An implementation swap is therefore a change to what gets created next, never a retroactive change to anyone's existing contract.

### Which code a legacy runs

| Created | Implementation | Pull vault |
|---|---|---|
| Since 2026-11-13 20:42 UTC | Legacy v3, `0xF9B6241Ab7513181C9Ed4F0920E0B008fB9264e9` | `LegacyPullVaultV2` |
| 2026-11-07 to 2026-11-13 | `0x2E2dE6475fcBE77E7930499981850d37b237924C` | `LegacyPullVaultV2` |
| Before 2026-11-07 | The implementation of its day (the earliest 22 are full contracts, not clones) | The first `LegacyPullVault`, or none for legacies older than it |

Read it yourself: an EIP-1167 clone's 45 bytes of code contain the address of its implementation. Nothing moves a legacy from one row to another; an owner who wants the newer code creates a new legacy (see [Moving an older legacy](#moving-an-older-legacy)).

## CREATE2 for a predictable, auditable address

10102 uses `CREATE2` to make the legacy contract's address predictable _before_ deployment. The address is derived from the deployer, a salt (the owner's EOA), and the initcode hash. Because every EOA legacy clone uses the same EIP-1167 initcode (parameterised only by the shared implementation address), the hash is stable across users and the address derivation is deterministic and auditable. This lets us:

- Show users the legacy address in the create form and have it match what actually gets deployed.
- Let beneficiaries verify the legacy address on Etherscan against a known pattern (every EOA legacy is a 45-byte EIP-1167 clone pointing at a published shared implementation address; easy to recognise once you've seen one).

The predicted address is deliberately _not_ what users grant token permissions to. Permissions name the pull vault, one published contract every wallet can verify, rather than a per-user address that has no code at signing time. The reasoning is in [How Legacy Creation Works](create-flow-v2.md).

## Why clones: the EIP-1167 refactor

Before the clone refactor, each EOA legacy was a full `TransferEOALegacy` deployment: ~6M gas per create, multiple dollars of mainnet fees at typical gas prices. Because every instance's code was byte-identical, paying to redeploy the same bytecode per user was pure waste.

Switching to EIP-1167 keeps the per-user storage isolation (each clone has its own state slots) while dropping the on-chain code to the ~45-byte forwarder stub. `createLegacy` now costs roughly 1.05M gas on mainnet, an 80%+ reduction. The mainnet cutover happened with Legacy #23 (the first clone-based creation); everything before that remains as an independently-deployed contract and continues to function normally.

The trade-off is that "bug in one user's legacy can't corrupt another's" now means _in storage_: the executable code is shared. The shared implementation does not touch the router's storage and has no mechanism to reach sibling clones, so the isolation we actually care about (one user's state is untouched by another's activity) is preserved. A new implementation can be set for **new** creations only, through the timelock; existing clones pin their implementation at creation and are never affected by a swap. The cost of that immutability is honest too: a bug in the implementation stays with the clones created from it (see the [Upgrade Policy](upgrade-policy.md) for how we handle that). And the implementations in use today are newer than the two audits: see [Review status](#review-status) below.

## Creating an EOA legacy

Creation is a single transaction, `TransferEOALegacyRouter.createLegacyV2WithMaxFee(...)`, that deploys the clone, registers the owner's Permit2 token permissions with the pull vault as spender, records the owner's terms consent, and refuses to go through if the delivery fee is higher than the one the owner was shown, atomically. The full mechanics (Permit2, the vault, transaction-based consent, PII-free events) have their own page: [How Legacy Creation Works](create-flow-v2.md). In outline:

1. The user configures beneficiaries, allocations, activation trigger (inactivity window in days), and included assets in the UI. Names and notes stay on the user's device; they are not transaction inputs.
2. The user signs one gas-free Permit2 batch for the included ERC-20s, then submits the create.
3. The Router deploys the clone via `LegacyDeployer` with a deterministic salt (CREATE2, so the address matches the prediction shown in the UI), registers the Permit2 batch, binds the owner to the new legacy in the vault, and records consent against the published terms hash.
4. The Router emits the (PII-free) create event. The subgraph creates the legacy entity.
5. The per-legacy contract initializes `lastActivityTimestamp` to the creation block, and copies the delivery fee rate of that moment.

What legacy v3 adds (NFTs, backup beneficiaries, an executor) is not part of the create: the owner sets each on the legacy's own page afterwards, with a call to the legacy contract itself. All three are off until then.

A legacy contract holds _rules_, not custody, with one small exception for ETH:

- **ERC-20**: named in the Permit2 permission signed at create. Tokens stay in the owner's wallet; on activation the claim path pulls _only what that permission allows, only on activation_. Tokens can also be added later with a small on-chain Permit2 approval.
- **ETH**: ETH cannot be permissioned like an ERC-20. The owner swaps ETH for a supported storage token (WETH, a liquid staking token) via the in-app Uniswap integration, and the resulting token is permissioned as above. This keeps the contract's permission surface uniform across assets.
- **NFTs** (legacy v3): stay in the owner's wallet too. The owner approves the pull vault as operator for a collection and names single items; see [NFTs](#nfts).

The owner can add or remove assets at any time before activation; removal means revoking the token's permission (directly in the wallet via Permit2, or `approve(0)` for pre-v2 direct approvals), or deleting and recreating the legacy.

## Heartbeat (reset activation timer)

Because the EOA has no on-chain code to hook into, the per-legacy contract tracks its own `lastActivityTimestamp`. These reset it:

- **Explicit heartbeat**: the owner clicks `I'm still alive` in the UI, which submits a tiny transaction to the Router. Cheap, unambiguous, on-chain. A Premium owner can sign the check-in instead and let our relay send it and pay the gas (see [Gas-Sponsored Intents](gas-sponsored-intents.md)).
- **Edit**: any on-chain edit to the legacy (beneficiaries, allocations, trigger) resets the timestamp as a side effect, because the edit is itself an outgoing transaction from the owner's EOA. On a legacy v3 the same goes for setting NFTs, backup beneficiaries or the executor.

_Not_ a heartbeat: arbitrary transactions from the owner's EOA that don't touch the legacy. The per-legacy contract has no way to see those, and by default nothing fills the gap: activity tracking is exactly "interactions with this legacy," nothing more. This is the deliberate trade-off of the EOA path: a fully on-chain inactivity timer with no external dependency, in exchange for the owner having to check in if they aren't otherwise editing the legacy. The one exception is opt-in: [automatic renewal](eoa-activity-auto-renew.md) (Premium), where an attestor renews the timer for an owner whose wallet has been active, within bounds the router enforces. See [Indexing & Activity Tracking](indexing-and-activity-tracking.md) for how this compares to the Safe path.

## Editing

Edits are single-EOA transactions, not multi-sig, so they're instant:

- Any field can change: beneficiaries, allocations, activation trigger, asset list. (Names and notes are stored on the owner's device and change without a transaction.)
- The Router emits the matching update event (`TransferEOALegacyDistributionUpdated`, `TransferEOALegacyTriggerUpdated`, …); the subgraph refreshes the entity.
- `lastActivityTimestamp` is reset as a side effect of the outgoing transaction.
- On a legacy v3, any transaction that sets the first line again clears the backup beneficiaries and the NFT list, also when the addresses and shares it sends are the same as before (the router's full-configuration edit always sets the first line), because both name first-line addresses. The contract emits events that say so, and the app warns before the save and tells the owner afterwards to set them again.

There's no notion of "co-signers need to approve the edit"; the whole point of an EOA legacy is that the owner has sole authority.

## Deleting

`TransferEOALegacyRouter.deleteLegacy(...)` does three things atomically at the protocol level, with a clearing step at the UI level:

1. Marks the legacy as deleted in the Router + per-legacy contract, and **releases the owner's binding in the pull vault**: after this, the Permit2 permissions signed at create can no longer be used by anything. The subgraph updates the entity status.
2. Returns any ETH held _inside_ the per-legacy contract (there's a small amount of dust possible from storage-token mechanics) to the owner.
3. The UI then walks the owner through clearing what is left in the wallet's name: the Permit2 entries that named the vault (Permit2's `lockdown`), and, for **pre-v2 legacies** that were set up with direct ERC-20 approvals, an `approve(legacyContract, 0)` per token with a live allowance. This is best-effort; a partial success still leaves the legacy deleted, and the app offers the rest again from Home.

Empty legacies (no tokens ever approved) skip the clearing entirely. It is strictly a "for your protection" cleanup; the deleted legacy contract can no longer pull tokens anyway, but dangling approvals are a general hygiene problem worth fixing.

## Activation

Activation is a single transaction any beneficiary can submit:

1. Beneficiary calls `TransferEOALegacyRouter.activeLegacy(legacyId, ...)` from an address matching one of the configured beneficiaries (primary, or a contingent whose window has elapsed).
2. The per-legacy contract evaluates the activation trigger entirely on-chain (`_lastTimestamp + lackOfOutgoingTxRange <= block.timestamp`), with no external call. There is no oracle and no off-chain activity lookup: the only "activity" the contract considers is the owner's interactions with this legacy (see [Heartbeat](#heartbeat-reset-activation-timer) above). If the owner hasn't touched the legacy for the configured window, the claim is allowed; otherwise it reverts.
3. If the check passes, the legacy iterates the allocations and moves each (asset, beneficiary) amount from the owner's EOA, per token, through the most generous rail available to that legacy: a direct ERC-20 allowance (pre-v2), a Permit2 permission naming the legacy itself, or a Permit2 permission through its pull vault (v2). Every generation of legacy stays claimable without migration, with one known exception: on a legacy created before 2026-11-07, USDT approved directly to the legacy contract (not through Permit2) makes a claim that lists it revert, and the other assets are claimed without it. No such approval existed on mainnet on 2026-11-02, and the app uses Permit2 for USDT on those legacies.
4. The Router emits `TransferEOALegacyActivated`; the subgraph marks the legacy activated.

Batches over 100 transfers are executed in chunks, with the remaining distribution callable via a `Claim Remaining Fund` follow-up; see the user guide for the beneficiary-side flow.

A beneficiary whose wallet holds no ETH can sign the claim instead and have it relayed; see [Gas-Sponsored Intents](gas-sponsored-intents.md).

## The delivery fee

When beneficiaries claim tokens or ETH, a fee is deducted from what is delivered. The rules are in the legacy contract, not in our servers:

- **The rate is copied into the legacy when it is created** (from `Payment`, 0.25% today) and never changes for that legacy. The create form shows it, and `createLegacyV2WithMaxFee` reverts if the rate on-chain is higher than the one shown.
- **At most 0.5% on legacies created since 2026-11-07**: the clone refuses to initialize with more (`MAX_ADMIN_FEE_BPS = 50`).
- **None on legacies created before 2026-09-29**, the day the fee took effect.
- **None on NFTs.** `claimNfts` takes nothing.
- **On a legacy v3 the fee reaches the treasury as the claimed token**, straight from the owner's wallet on the same rail as the payout, never converted. If that transfer fails (a token that blocks the treasury, say), the beneficiaries are paid all the same and the fee is simply not collected. Legacies created from 2026-09-29 to 2026-11-13 convert a token fee to ETH on the way, with no minimum price, and send the token itself if the conversion fails; a bad price there costs the treasury, never a beneficiary.

Timelocks and gifts carry no delivery fee. The Shield Vault has its own rule: see [Shield Vault](shield-vault.md#the-fee).

## Legacy v3

Every wallet legacy created since 2026-11-13 20:42 UTC is a clone of `TransferEOALegacyV3` (`contracts/forwarding/TransferLegacyEOAContractV3.sol`). It does everything above unchanged, and adds three optional features. The owner sets them by calling the legacy contract directly; check-ins and token claims still go through the router.

The steps in the app are in the user guide: [NFTs in a Legacy](../user-guide/legacy/nfts-in-a-legacy.md), [Backup Beneficiaries and Backup Heirs](../user-guide/premium-features/manage-contingent-beneficiaries.md), [Name an Executor](../user-guide/premium-features/executor.md) and [Move a Legacy to the New Version](../user-guide/legacy/move-to-the-new-version.md). This page is what the contract enforces.

### NFTs

- **Named items, one beneficiary each.** `setNftItems` stores up to 50 items: a collection, a token id, an amount (1 for an ERC-721, any amount of one id for an ERC-1155) and the first-line beneficiary it goes to. An NFT is never split.
- **Only collections on the list.** The collection must be on `NftCollectionWhiteList` when the item is named. The list is **not** read at claim time, so removing a collection later stops new naming only: what was already named stays claimable, until its owner saves the NFT list again or changes the first-line beneficiaries or shares, either of which drops an item of a removed collection for good.
- **They stay in the wallet.** The owner approves `LegacyPullVaultV2` as operator for the collection (one `setApprovalForAll`). At claim time the legacy asks the vault to move the item, and the vault does it only for the legacy bound to that owner.
- **Claiming.** The beneficiary calls `claimNfts` on the legacy itself, from their own wallet (there is no signed, relayed form of this call, so it needs a little ETH for gas). On the first line each item goes to the beneficiary it names, or, with backups on, to that beneficiary's backup in its window. A second- or third-line beneficiary, in its period, takes the items still unclaimed; with backups on, only those of shares nobody else has started to claim. An item that cannot move at that moment (the wallet no longer holds it, the approval is gone, the collection has paused transfers) is skipped and stays claimable later; an ERC-1155 item can move in parts. A claim that moves nothing reverts when it would have been the claim that activates the legacy, so an empty claim never activates it; on a legacy already activated it simply moves nothing.
- **No fee.**

On mainnet the list holds three collections since 2026-11-16:

| Collection | Address | Worth knowing |
|---|---|---|
| CryptoPunks, in the ERC-721 wrapper "CryptoPunks 721" | `0x000000000000003607fce1aC9e043a86675C5C2F` | The original 2017 CryptoPunks contract is not ERC-721 and the list refuses it, as it does the older "Wrapped Cryptopunks". A punk can be named only once it is in this wrapper, and unwrapping a named punk leaves nothing to claim for that item. |
| Bored Ape Yacht Club | `0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D` | Ownership renounced, no pause. |
| Pudgy Penguins | `0xBd3531dA5CF5857e7CfAA92426877b022e612cf8` | The collection's own owner key can pause every transfer. While paused, a named penguin is skipped by a claim and stays claimable once the pause ends. Nobody at 10102 can lift that pause. |

Each was walked to a claim on a fork of mainnet before it was added. Being on the list says one thing: an item of that collection could be moved to its beneficiary when we checked. It is no endorsement and no valuation, and the list can change. Its admins are the maintainer key and the governance Safe; read it with `getCollections()`.

### Backup beneficiaries

A Premium feature, called backup heirs in the app. `setBackupLines(backupDelay, lines)` gives a first-line beneficiary up to two backups for their own share:

- The beneficiary can claim their share from the activation date. Their first backup can claim it one wait later, the second backup two waits later. The owner chooses the wait, from 30 to 3,650 days.
- **Each share is its own account.** A claim pays that share's percentage of everything a token has made available, minus what the share already received. So the order of claims changes nothing for anyone else, and nobody waits for another beneficiary.
- **A share belongs to the first address that receives anything from it.** From then on only that address receives from that share, later arrivals included. A backup cannot take a share its beneficiary has started claiming.
- With backups on, beneficiaries receive tokens and ETH as they are; the claim that converts a stored token back to ETH is refused.
- A backup can never be the owner, the executor, or anyone already named on another line. The contract checks this when backups are set and again on later edits.
- The second and third line (the legacy-wide contingent beneficiaries) still exist. In this mode they take only the shares nobody has claimed, and the contract requires the second line to open no earlier than two waits after the claim date, the moment the second backup's window opens.

Honest limit, as the terms state it: no one checks *why* a share was not claimed. Once the wait has passed, the backup can claim, and a claim cannot be undone.

### An executor

Also a Premium feature. `setExecutor` names one address that can call `attestDeath()`:

- **Only after a real silence.** The call is refused until the owner has been silent for half the activation trigger, and refused once the legacy is claimable anyway.
- **What it does.** Claims open 30 days after the attestation, if that is sooner than the normal date. It never opens them later, and at most one attestation counts per 30 days.
- **Any owner activity cancels it.** A check-in, an edit, any v3 setting: the attestation no longer counts and the normal date applies again. The reminder worker emails the owner when an attestation lands, if the owner saved an email, because the 30 days only help an owner who hears about it. For an owner on automatic renewal, the attestor renews after an attestation only on a transaction the wallet sent after it.
- **The executor gets nothing.** The address can never be a beneficiary or a backup, never holds or receives anything, and is never needed for a claim: without an attestation, claims open on the normal date.

Honest limit: **nobody verifies the attestation.** Neither the contract nor 10102 checks that the owner has died. A dishonest executor can bring the claim date forward to 30 days out, once the owner has been silent for half their period, and the only defence is the owner's own check-in. "Executor" is a role in this contract only; it appoints no one under any law.

### Moving an older legacy

There is no upgrade of a legacy in place. The app's **move to the new version** is a delete plus a create, guided:

- The old legacy is deleted and a new one is created with the same beneficiaries, shares and check-in period, and the same tokens are permissioned again. Wallets that can batch do it in one transaction; others confirm each step. Before anything is removed, the app simulates the removal and the create together and stops if it cannot check.
- **What changes**: a new contract address (printed claim cards and saved links must be made again), the check-in timer starts again, the delivery fee is the rate in force on the day of the move, and reminders and automatic renewal must be turned on again. Relayed (gas-free) claims are allowed on every new legacy; an owner who had switched them off switches them off again.
- Nothing obliges an owner to move. An older legacy keeps working exactly as it was created.

### Review status

Stated plainly: legacy v3 has had internal reviews and automated testing, and went live before an external review, which is still to be commissioned. The clone used from 2026-11-07 to 2026-11-13 and `LegacyPullVaultV2` had internal adversarial review and have not had a published audit. The two audits (reports of January 2025 and October 2025) examined the legacy contracts as they were then, before wallet legacies were clones. A clone cannot be upgraded, so a finding would mean a new implementation for new legacies and a notice to the owners of existing ones. See [Review status](README.md#review-status).

## No external dependency at activation

EOA activation has no off-chain dependency: the eligibility decision is a single on-chain comparison inside the per-legacy contract, so it works even if our app, backend, or indexer are entirely down. The flip side is that the inactivity timer is only ever reset by the owner's own interactions with the legacy; unless the owner opted in to automatic renewal, nothing watches the owner's broader wallet activity. An owner who is active elsewhere but never touches the legacy will still see the timer count down, which is why the [heartbeat](#heartbeat-reset-activation-timer) exists. Safe-owned legacies get a softer version of this: their Safe Guard records activity on every Safe transaction, so normal wallet use keeps the legacy fresh automatically.

## Why split EOA and Safe paths at the contract level

The two flows share 80% of their semantics, but the 20% that differs (how activity is tracked, who can authorize edits, how activation executes) is different enough that conflating them would force both paths to carry the other's complexity. Two thin routers with clear contracts are easier to audit and easier to upgrade in isolation.
