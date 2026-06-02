---
description: >-
  Subgraphs on The Graph keep the UI fast; activity tracking is a fully
  on-chain inactivity timer, different for Safe and EOA owners.
---

# Indexing & Activity Tracking

Two distinct problems sit under one banner here:

1. **Indexing** — reading structured data out of on-chain events quickly. Solved with [The Graph](https://thegraph.com).
2. **Activity tracking** — knowing whether an owner has been inactive long enough to trigger activation. Solved entirely on-chain, with no oracle: a Safe Guard for Safe owners, and a per-legacy inactivity timer for EOA owners.

The indexing layer is strictly additive: the on-chain contracts are authoritative, and the UI falls back to direct RPC reads when the indexed layer is stale or unavailable. The activation decision never depends on it.

> **Retired:** earlier drafts of this product described EOA activity tracking as a "Chainlink Functions + Moralis hybrid" that bridged an off-chain check of the owner's wider wallet activity back on-chain. That mechanism is **not in use** (the Chainlink Automation/Functions integration was retired and its subscriptions cancelled on mainnet and Sepolia). EOA activation relies solely on the on-chain inactivity timer described below.

## Subgraphs on The Graph

We maintain a single subgraph per chain:

- **Legacy + timelock + reminders subgraph** — indexes every event emitted by the routers: `LegacyCreated`, `LegacyUpdated`, `LegacyDeleted`, `Activated`, `TimelockCreated`, `ReminderConfigured`, plus the Safe's own `ChangeGuard` / `EnableModule` / `AddedOwner` / `ChangedThreshold` for 10102-enabled Safes. The UI reads the bulk of its state from here.

Earlier versions also ran a separate "token balance" subgraph for the "your assets" pickers during legacy creation, and an "ETH totals" subgraph for system-wide aggregate metrics. Both were retired in v2026.05.15 once the equivalent reads were moved on-chain:

- Token balances are now fetched via viem against the canonical `TokenWhitelist` contract plus per-token ERC-20 `balanceOf` (`src/services/web3-assets-service.ts` in `computing`).
- System-wide TVL is computed by the admin panel, which walks the legacy/timelock subgraph for the entity set and then reads balances directly via `Multicall3.getEthBalance` plus per-token ERC-20 `balanceOf` / `allowance`.

The remaining subgraph stays narrow, indexing time stays bounded, and freshness-critical reads bypass it entirely.

### Mainnet and Sepolia

Every subgraph is deployed on both mainnet and Sepolia. Deployment IDs and endpoint URLs are configuration; the app picks the right set based on the connected chain.

### Freshness and the on-chain fallback

Subgraphs are eventually-consistent. After a transaction is mined, it typically takes a handful of blocks for an event to appear in subgraph responses. During that window, the UI cannot trust "legacy not found in subgraph" as ground truth — a legacy that was just created would be falsely reported as nonexistent.

The app handles this in two places:

- **Post-mutation freshness** — hooks like `useHasExistingLegacy` do a direct on-chain `readContract` call via viem with `blockTag: 'latest'` immediately after a mutation, so the UI state reflects the latest mined state without waiting for the subgraph.
- **Retry loop** — if an on-chain read returns an unexpected state (for example, the legacy still appears after a deletion), the hook retries a few times with short backoffs to let the node catch up.

The net result: subgraphs give us speed, and on-chain reads give us correctness when speed isn't enough.

## Activity tracking

Solidity cannot directly read "the timestamp of `wallet.address`'s most recent outgoing transaction." The EVM doesn't expose it. So instead of trying to observe an owner's _general_ wallet activity, both legacy paths track an activity timestamp that the contracts can update natively — the difference is only in _what_ counts as activity.

### Safe owners — the Safe Guard

A smart-contract wallet can track activity internally. When the owner is a Safe, we install a **Safe Guard** (see the [Safe Guard integration](legacy-contracts-created-with-safe-sdk.md)) that records the timestamp of every Safe transaction. Any normal Safe activity — DeFi, staking, voting — implicitly keeps the legacy fresh, with no deliberate check-in. At activation time the Router reads that timestamp directly from the Guard and compares it against the configured inactivity window. Fully on-chain, no oracle.

### EOA owners — a per-legacy inactivity timer

A plain EOA has no on-chain code to hook into, so an EOA legacy tracks activity on the only surface it controls: **itself**. Each EOA legacy stores a `_lastTimestamp` (last-activity timestamp) and a `lackOfOutgoingTxRange` (the configured inactivity window). The eligibility check is a single on-chain comparison with no external call:

```solidity
// TransferEOALegacy._checkActiveLegacy()
if (_lastTimestamp + lackOfOutgoingTxRange > block.timestamp) {
  return false; // still considered active — activation not allowed
}
return true;     // inactive long enough — eligible
```

`_lastTimestamp` is reset to `block.timestamp` by any owner interaction with the legacy: creation, edits (beneficiaries / allocations / trigger / name), withdrawals, the auto-swap / unswap helpers, receiving ETH directly from the owner, or the explicit `activeAlive` ("I'm still alive") heartbeat. A beneficiary then activates permissionlessly via `TransferEOALegacyRouter.activeLegacy(legacyId, ...)`, and the contract itself decides whether the window has elapsed.

The deliberate limitation: arbitrary transactions the owner makes _elsewhere_ are invisible to the per-legacy contract. There is **no oracle and no off-chain lookup** bridging that information in. An EOA owner who is active on-chain but never touches their legacy will still see the timer count down — which is exactly why the heartbeat exists.

### Trust and verification

- **The contract is authoritative and self-contained.** Both paths reduce activation to an on-chain timestamp comparison. Nothing off-chain can fabricate a "yes, activate" signal or block a legitimate activation.
- **No external party in the trust path.** There is no oracle network, no third-party activity API, and no backend attestation involved in deciding whether a legacy may activate.

### Failure modes

Because activation is purely on-chain, an outage of our app, backend, or the subgraph has **no effect on the ability to activate** — a beneficiary can call the router directly. The realistic EOA failure mode runs the other way: the owner forgets to check in and the timer elapses while they're still alive. That is recoverable (they heartbeat or edit to reset it), whereas a premature disbursement would not be — so the design intentionally only ever resets the timer on the owner's own deliberate, on-chain interactions.

## What this means operationally

- Activation never waits on us. There is no activity oracle to be "down," and no subscription that can run dry and block a claim.
- The off-chain pieces that _do_ exist around the legacy — the [reminder worker](email-reminders.md) and the subgraph — are reminder/indexing conveniences only. If they degrade, owners may miss a nudge or the UI may lag, but correctness and the ability to claim are unaffected.
- Moving toward "every wallet is a smart wallet" (EIP-4337 account abstraction) would let EOA owners get the same passive, broad activity tracking Safe owners enjoy today; we expect to add an AA-specific integration once such wallets standardize a way to expose it.
