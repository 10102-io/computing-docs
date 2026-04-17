---
description: >-
  How Transfer legacies work when the owner is a plain EOA (MetaMask, Ledger,
  Trezor, …) with no Safe involvement, and how CREATE2 makes the setup flow
  feel less alarming.
---

# Legacy Contracts Created with EOAs

An **externally owned account (EOA)** is an Ethereum account controlled by a single private key — the wallet most people have in their browser or hardware device. EOAs have no on-chain code, which makes them elegant but creates one genuine constraint for inheritance: **there is no built-in way for a smart contract to read an EOA's last outgoing transaction timestamp**. The 10102 EOA flow is shaped around that constraint.

This page describes the pure-EOA **Transfer legacy** path. Multisig legacies are Safe-only; Safe-backed Transfer legacies are covered in [Legacy Contracts Created with Safe SDK](legacy-contracts-created-with-safe-sdk.md).

## Contract roles

| Contract | Purpose |
|---|---|
| `TransferEOALegacyRouter` | Creates, edits, deletes, and activates EOA-owner Transfer legacies. |
| `LegacyDeployer` | CREATE2 factory. Deploys per-user legacy contracts to a deterministic address. |
| Per-legacy `TransferEOALegacyContract` | Stores beneficiaries, allocations, approved asset list, activation trigger, last-activity timestamp. |

Routers are upgradeable behind proxies; per-legacy contracts are minimal (storage + a few entry points) and not upgradeable.

## CREATE2 for a better first-time experience

The first time a user creates a legacy, their per-legacy contract does not yet exist on-chain. A naive flow would require the user to (1) deploy the contract, then (2) approve it as a spender for each ERC-20 they want to include. The second step, targeting a freshly-deployed contract, tends to trigger wallet "suspicious transaction" warnings — the approval target has no on-chain history, which is a legitimate heuristic against scams but bad UX for legitimate tooling.

10102 uses `CREATE2` to make the legacy contract's address predictable _before_ deployment. The address is derived from the deployer, a salt (the owner's EOA), and the initcode hash. We explicitly preserve the same initcode across users so that the address derivation is stable and auditable. This lets us:

- Deploy the contract and approve it in two separate transactions without the approval target looking "unknown" to wallets that track our contract family.
- Show users the legacy address at step 1 and have it match what actually gets deployed.
- Let beneficiaries verify the legacy address on Etherscan against a known pattern.

## Creating an EOA legacy

The flow is intentionally split into two explicit signer actions, because a single atomic flow is not possible without EIP-5792 `wallet_sendCalls` support (something we're tracking for a future release).

### Step 1 — Deploy the legacy contract

1. The user configures beneficiaries, allocations, activation trigger (inactivity window in days), and name/note in the UI.
2. The user submits `TransferEOALegacyRouter.createLegacy(...)` with those parameters.
3. The Router calls `LegacyDeployer.deploy(...)` with a deterministic salt. The CREATE2 deploy produces the per-legacy contract at a predictable address.
4. The Router emits `LegacyCreated`. The subgraph creates the legacy entity.
5. The per-legacy contract initializes `lastActivityTimestamp` to the creation block.

### Step 2 — Include assets

A legacy contract holds _rules_, not custody — with one small exception for ETH. For each asset the owner wants included:

- **ERC-20** — the owner submits an `approve(legacyContract, amount)` to the token contract. The subgraph indexes the approval against the legacy. On activation, the Router / per-legacy contract uses `transferFrom` to move tokens from the owner's EOA to beneficiary addresses — _only the pre-approved amount, only on activation_.
- **ETH** — ETH cannot be approved like an ERC-20. The owner swaps ETH for a supported storage token (WETH, a liquid staking token) via the in-app Uniswap integration, then approves the storage token as above. This keeps the contract's permission surface uniform across assets.

The owner can add or remove assets at any time before activation. Removing an asset means approving 0, or deleting and recreating the legacy.

## Heartbeat (reset activation timer)

Because the EOA has no on-chain code to hook into, the per-legacy contract tracks its own `lastActivityTimestamp`. Two things reset it:

- **Explicit heartbeat** — the owner clicks `I'm still alive` in the UI, which submits a tiny transaction to the Router. Cheap, unambiguous, on-chain.
- **Edit** — any edit to the legacy (beneficiaries, allocations, trigger, name) resets the timestamp as a side effect, because the edit is itself an outgoing transaction from the owner's EOA.

_Not_ a heartbeat: arbitrary transactions from the owner's EOA that don't touch the legacy. The per-legacy contract has no way to see those. This is the trade-off of the EOA path — the [Chainlink/Moralis hybrid](indexing-and-activity-tracking.md) fills in the gap at activation time.

## Editing

Edits are single-EOA transactions, not multi-sig, so they're instant:

- Any field can change — name, beneficiaries, allocations, activation trigger, asset list.
- The Router emits an `LegacyUpdated` event; the subgraph refreshes the entity.
- `lastActivityTimestamp` is reset as a side effect of the outgoing transaction.

There's no notion of "co-signers need to approve the edit" — the whole point of an EOA legacy is that the owner has sole authority.

## Deleting

`TransferEOALegacyRouter.deleteLegacy(...)` does three things atomically at the protocol level, with an optional revoke loop at the UI level:

1. Marks the legacy as deleted in the Router + per-legacy contract. The subgraph updates the entity status.
2. Returns any ETH held _inside_ the per-legacy contract (there's a small amount of dust possible from storage-token mechanics) to the owner.
3. The UI then walks the owner through a sequence of `approve(legacyContract, 0)` calls — one per previously-approved token — to revoke spender permissions. This is best-effort; a partial success still leaves the legacy deleted.

Empty legacies (no tokens ever approved) skip the revoke loop entirely. The revoke loop is strictly a "for your protection" cleanup; the deleted legacy contract can no longer call `transferFrom` anyway, but dangling approvals are a general hygiene problem worth fixing.

## Activation

Activation is a single transaction any beneficiary can submit:

1. Beneficiary calls `TransferEOALegacyRouter.activate(legacyId)` from an address matching one of the configured beneficiaries (primary, or a contingent whose window has elapsed).
2. The Router checks the activation trigger by consulting the [Chainlink Functions + Moralis hybrid](indexing-and-activity-tracking.md) — because the per-legacy contract's `lastActivityTimestamp` only sees activity _on the legacy_, not general wallet activity, the Router additionally verifies the owner's EOA has had no outgoing transactions for the configured window using an off-chain activity check bridged back on-chain via Chainlink.
3. If the check passes, the Router iterates the allocations and issues `transferFrom(owner, beneficiary_i, amount_i)` for each (asset, beneficiary) pair.
4. The Router emits an `Activated` event; the subgraph marks the legacy activated.

Batches over 100 transfers are executed in chunks, with the remaining distribution callable via a `Claim Remaining Fund` follow-up — see the user guide for the beneficiary-side flow.

## When the activity oracle is unavailable

If Chainlink or Moralis are down, activation is not possible for EOA legacies — the Router refuses to move assets without a current activity verdict. This is a deliberate safety property: we'd rather a legitimate claim be delayed than risk activating with stale activity data. Safe-owned legacies don't have this dependency because their Safe Guard already holds fresh activity on-chain.

## Why split EOA and Safe paths at the contract level

The two flows share 80% of their semantics, but the 20% that differs — how activity is tracked, who can authorize edits, how activation executes — is different enough that conflating them would force both paths to carry the other's complexity. Two thin routers with clear contracts are easier to audit and easier to upgrade in isolation.
