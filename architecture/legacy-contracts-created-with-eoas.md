---
description: >-
  How Transfer legacies work when the owner is a plain EOA (MetaMask, Ledger,
  Trezor, …) with no Safe involvement: clones, the single-transaction create,
  heartbeat, editing, deletion, and activation.
---

# Legacy Contracts Created with EOAs

An **externally owned account (EOA)** is an Ethereum account controlled by a single private key, the wallet most people have in their browser or hardware device. EOAs have no on-chain code, which makes them elegant but creates one genuine constraint for legacy plans: **there is no built-in way for a smart contract to read an EOA's last outgoing transaction timestamp**. The 10102 EOA flow is shaped around that constraint.

This page describes the pure-EOA **Transfer legacy** path. Multisig legacies are Safe-only; Safe-backed Transfer legacies are covered in [Legacy Contracts Created with Safe SDK](legacy-contracts-created-with-safe-sdk.md).

## Contract roles

| Contract | Purpose |
|---|---|
| `TransferEOALegacyRouter` | Creates, edits, deletes, and activates EOA-owner Transfer legacies. Holds the address of the shared `TransferEOALegacy` implementation used by all clones. |
| `LegacyDeployer` | CREATE2 factory. Deploys per-user legacy contracts to a deterministic address. For EOA legacies this deploys an EIP-1167 minimal proxy (~45 bytes of on-chain code) that delegates to the shared implementation. |
| Shared `TransferEOALegacy` implementation | The actual contract logic. One deployment per network; every EOA legacy clone delegates to it. |
| Per-legacy clone | A thin EIP-1167 proxy at a CREATE2-predictable address. Stores beneficiaries, allocations, included asset list, activation trigger, and last-activity timestamp in its own storage; executes shared code via `DELEGATECALL`. |
| `LegacyPullVault` | Immutable, admin-free singleton that acts as the single Permit2 spender for all v2 creates. Holds a first-write-wins `owner → legacy` binding; only an owner's own bound legacy can pull their tokens. See [How Legacy Creation Works](create-flow-v2.md). |

Routers are upgradeable behind transparent proxies (see the [Upgrade Policy](upgrade-policy.md)). Per-legacy clones are _not_ upgradeable at all: **each clone pins its implementation and its vault at creation time**. The router's `_codeAdmin` can point *new* clones at a new shared implementation, but existing clones keep the exact code they were created with, forever. An implementation swap is therefore a change to what gets created next, never a retroactive change to anyone's existing contract.

## CREATE2 for a predictable, auditable address

10102 uses `CREATE2` to make the legacy contract's address predictable _before_ deployment. The address is derived from the deployer, a salt (the owner's EOA), and the initcode hash. Because every EOA legacy clone uses the same EIP-1167 initcode (parameterised only by the shared implementation address), the hash is stable across users and the address derivation is deterministic and auditable. This lets us:

- Show users the legacy address in the create form and have it match what actually gets deployed.
- Let beneficiaries verify the legacy address on Etherscan against a known pattern (every EOA legacy is a 45-byte EIP-1167 clone pointing at the published shared implementation address; easy to recognise once you've seen one).

The predicted address is deliberately _not_ what users grant token permissions to. Permissions name the `LegacyPullVault`, one published contract every wallet can verify, rather than a per-user address that has no code at signing time. The reasoning is covered in [How Legacy Creation Works](create-flow-v2.md).

## Why clones: the EIP-1167 refactor

Before the clone refactor, each EOA legacy was a full `TransferEOALegacy` deployment: ~6M gas per create, multiple dollars of mainnet fees at typical gas prices. Because every instance's code was byte-identical, paying to redeploy the same bytecode per user was pure waste.

Switching to EIP-1167 keeps the per-user storage isolation (each clone has its own state slots) while dropping the on-chain code to the ~45-byte forwarder stub. `createLegacy` now costs roughly 1.05M gas on mainnet, an 80%+ reduction. The mainnet cutover happened with Legacy #23 (the first clone-based creation); everything before that remains as an independently-deployed contract and continues to function normally.

The trade-off is that "bug in one user's legacy can't corrupt another's" now means _in storage_: the executable code is shared. In practice the shared implementation is a small, audited contract that does not touch the router's storage and has no mechanism to reach sibling clones, so the isolation guarantee we actually care about (one user's state is untouched by another's activity) is preserved. The `_codeAdmin`-controlled pointer lets us patch the shared logic for **new** creations; existing clones pin their implementation at creation and are never affected by a swap. The cost of that immutability is honest too: a bug in the implementation stays with the clones created from it (see the [Upgrade Policy](upgrade-policy.md) for how we handle that).

## Creating an EOA legacy

Creation is a single transaction, `TransferEOALegacyRouter.createLegacyV2(...)`, that deploys the clone, registers the owner's Permit2 token permissions with the `LegacyPullVault` as spender, and records the owner's terms consent, atomically. The full mechanics (Permit2, the vault, transaction-based consent, PII-free events) have their own page: [How Legacy Creation Works](create-flow-v2.md). In outline:

1. The user configures beneficiaries, allocations, activation trigger (inactivity window in days), and included assets in the UI. Names and notes stay on the user's device; they are not transaction inputs.
2. The user signs one gas-free Permit2 batch covering the included ERC-20s, then submits `createLegacyV2(...)`.
3. The Router deploys the clone via `LegacyDeployer` with a deterministic salt (CREATE2, so the address matches the prediction shown in the UI), registers the Permit2 batch, binds the owner to the new legacy in the vault, and records consent against the published terms hash.
4. The Router emits the (PII-free) create event. The subgraph creates the legacy entity.
5. The per-legacy contract initializes `lastActivityTimestamp` to the creation block.

A legacy contract holds _rules_, not custody, with one small exception for ETH:

- **ERC-20**: covered by the Permit2 permission signed at create. Tokens stay in the owner's wallet; on activation the claim path pulls _only what the permission covers, only on activation_. Tokens can also be added later with a small on-chain Permit2 approval.
- **ETH**: ETH cannot be permissioned like an ERC-20. The owner swaps ETH for a supported storage token (WETH, a liquid staking token) via the in-app Uniswap integration, and the resulting token is permissioned as above. This keeps the contract's permission surface uniform across assets.

The owner can add or remove assets at any time before activation; removal means revoking the token's permission (directly in the wallet via Permit2, or `approve(0)` for pre-v2 direct approvals), or deleting and recreating the legacy.

## Heartbeat (reset activation timer)

Because the EOA has no on-chain code to hook into, the per-legacy contract tracks its own `lastActivityTimestamp`. Two things reset it:

- **Explicit heartbeat**: the owner clicks `I'm still alive` in the UI, which submits a tiny transaction to the Router. Cheap, unambiguous, on-chain.
- **Edit**: any on-chain edit to the legacy (beneficiaries, allocations, trigger) resets the timestamp as a side effect, because the edit is itself an outgoing transaction from the owner's EOA.

_Not_ a heartbeat: arbitrary transactions from the owner's EOA that don't touch the legacy. The per-legacy contract has no way to see those, and there is **no oracle that fills the gap**: activity tracking is exactly "interactions with this legacy," nothing more. This is the deliberate trade-off of the EOA path: a fully on-chain inactivity timer with no external dependency, in exchange for the owner having to check in if they aren't otherwise editing the legacy. See [Indexing & Activity Tracking](indexing-and-activity-tracking.md) for how this compares to the Safe path.

## Editing

Edits are single-EOA transactions, not multi-sig, so they're instant:

- Any field can change: beneficiaries, allocations, activation trigger, asset list. (Names and notes are stored on the owner's device and change without a transaction.)
- The Router emits an `LegacyUpdated` event; the subgraph refreshes the entity.
- `lastActivityTimestamp` is reset as a side effect of the outgoing transaction.

There's no notion of "co-signers need to approve the edit"; the whole point of an EOA legacy is that the owner has sole authority.

## Deleting

`TransferEOALegacyRouter.deleteLegacy(...)` does three things atomically at the protocol level, with an optional revoke loop at the UI level:

1. Marks the legacy as deleted in the Router + per-legacy contract, and **releases the owner's binding in the `LegacyPullVault`**: after this, the Permit2 permissions signed at create can no longer be used by anything. The subgraph updates the entity status.
2. Returns any ETH held _inside_ the per-legacy contract (there's a small amount of dust possible from storage-token mechanics) to the owner.
3. For **pre-v2 legacies** that were set up with direct ERC-20 approvals, the UI then walks the owner through a sequence of `approve(legacyContract, 0)` calls (one per token with a live allowance) to revoke spender permissions. This is best-effort; a partial success still leaves the legacy deleted.

Empty legacies (no tokens ever approved) skip the revoke loop entirely. The revoke loop is strictly a "for your protection" cleanup; the deleted legacy contract can no longer pull tokens anyway, but dangling approvals are a general hygiene problem worth fixing. v2 owners who want zero residue can additionally clear the Permit2 allowance entries from their wallet (Permit2 supports mass revocation via `lockdown`).

## Activation

Activation is a single transaction any beneficiary can submit:

1. Beneficiary calls `TransferEOALegacyRouter.activeLegacy(legacyId, ...)` from an address matching one of the configured beneficiaries (primary, or a contingent whose window has elapsed).
2. The per-legacy contract evaluates the activation trigger entirely on-chain (`_lastTimestamp + lackOfOutgoingTxRange <= block.timestamp`), with no external call. There is no oracle and no off-chain activity lookup: the only "activity" the contract considers is the owner's interactions with this legacy (see [Heartbeat](#heartbeat-reset-activation-timer) above). If the owner hasn't touched the legacy for the configured window, the claim is allowed; otherwise it reverts.
3. If the check passes, the Router iterates the allocations and moves each (asset, beneficiary) amount from the owner's EOA, per token, through the most generous rail available to that legacy: a direct ERC-20 allowance (pre-v2), a Permit2 permission naming the legacy itself, or a Permit2 permission through the `LegacyPullVault` (v2). Every generation of legacy stays claimable without migration.
4. The Router emits an `Activated` event; the subgraph marks the legacy activated.

Batches over 100 transfers are executed in chunks, with the remaining distribution callable via a `Claim Remaining Fund` follow-up; see the user guide for the beneficiary-side flow.

## No external dependency at activation

EOA activation has no off-chain dependency: the eligibility decision is a single on-chain comparison inside the per-legacy contract, so it works even if our app, backend, or indexer are entirely down. The flip side is that the inactivity timer is only ever reset by the owner's own interactions with the legacy; there is no oracle watching the owner's broader wallet activity. An owner who is active elsewhere but never touches the legacy will still see the timer count down, which is why the [heartbeat](#heartbeat-reset-activation-timer) exists. Safe-owned legacies get a softer version of this: their Safe Guard records activity on every Safe transaction, so normal wallet use keeps the legacy fresh automatically.

## Why split EOA and Safe paths at the contract level

The two flows share 80% of their semantics, but the 20% that differs (how activity is tracked, who can authorize edits, how activation executes) is different enough that conflating them would force both paths to carry the other's complexity. Two thin routers with clear contracts are easier to audit and easier to upgrade in isolation.
