---
description: >-
  Subgraphs on The Graph keep the UI fast; activity tracking is an on-chain
  inactivity timer, different for Safe and EOA owners, with an optional
  attested auto-renew path for EOAs.
---

# Indexing & Activity Tracking

Two distinct problems sit under one banner here:

1. **Indexing**: reading structured data out of on-chain events quickly. Solved with [The Graph](https://thegraph.com).
2. **Activity tracking**: knowing whether an owner has been inactive long enough to trigger activation. The activation decision itself is entirely on-chain: a Safe Guard for Safe owners, and a per-legacy inactivity timer for EOA owners. EOA owners can additionally opt in to [auto-renew](eoa-activity-auto-renew.md), where an attestor resets the timer for them within strict on-chain bounds.

The indexing layer is strictly additive: the on-chain contracts are authoritative, and the UI falls back to direct RPC reads when the indexed layer is stale or unavailable. The activation decision never depends on it.

> **Retired:** earlier drafts of this product described EOA activity tracking as a "Chainlink Functions + Moralis hybrid" that bridged an off-chain check of the owner's wider wallet activity back on-chain. That mechanism is **not in use** (the Chainlink Automation/Functions integration was retired and its subscriptions cancelled on mainnet and Sepolia). Its successor is the strictly opt-in [auto-renew attestor](eoa-activity-auto-renew.md), a much narrower mechanism whose safety bounds live in the contract, not in an oracle network. Without opting in, EOA activation relies solely on the on-chain inactivity timer described below.

## Subgraphs on The Graph

We maintain a single subgraph per chain:

- **Legacy + timelock + reminders subgraph**: indexes the events of the legacy routers (creations, edits, deletions, activations, check-ins, automatic renewals and the router's own settings), the timelock vaults (`TimelockCreated`, withdrawals), the Premium contracts (purchases, coupon and referrer settings, reminder and watcher configuration, the PII-free notify events), the pull vault's bindings, the terms verifier, and the Safe's own `ChangedGuard` / `EnabledModule` / `AddedOwner` / `ChangedThreshold` for 10102-enabled Safes. The UI reads the bulk of its legacy and timelock state from here.

Since 2026-11-10 (published on both networks 2026-11-11) the same subgraph also indexes the following, next to the automatic-renewal, router-setting, coupon, pull-vault and terms events in the list above, which arrived in the same release:

- **Each legacy v3 clone**, through a per-clone template: NFT items set and claimed, backup beneficiaries set, per-share claims, the executor and an attestation, and the activation a legacy emits itself when an NFT claim is what activates it (that path does not pass through the router).
- **Both Shield Vaults** (v2 and v3): holdings opened, deposits, check-ins, withdrawals and changes requested, cancelled and applied, stops with the recovery sheet, releases, fees and owed shares.

Indexed is not the same as used by the app: for legacy v3 and for Shield Vault holdings **the app reads from the chain**, not from the subgraph. The v3 screens are gated on the implementation address in the clone's own code, and every figure on them (items, backups, windows, what a share has received) is a contract read. The subgraph entities are there for anyone querying the public endpoint.

Earlier versions also ran a separate "token balance" subgraph for the "your assets" pickers during legacy creation, and an "ETH totals" subgraph for system-wide aggregate metrics. Both were retired in v2026.05.15 once the equivalent reads were moved on-chain:

- Token balances are now fetched via viem against the canonical `TokenWhitelist` contract plus per-token ERC-20 `balanceOf` (`src/services/web3-assets-service.ts` in `computing`).
- System-wide TVL is computed by the admin panel, which walks the legacy/timelock subgraph for the entity set and then reads balances directly via `Multicall3.getEthBalance` plus per-token ERC-20 `balanceOf` / `allowance`.

The remaining subgraph stays narrow, indexing time stays bounded, and freshness-critical reads bypass it entirely.

### Mainnet and Sepolia

Every subgraph is deployed on both mainnet and Sepolia. Deployment IDs and endpoint URLs are configuration; the app picks the right set based on the connected chain.

### Freshness and the on-chain fallback

Subgraphs are eventually-consistent. After a transaction is mined, it typically takes a handful of blocks for an event to appear in subgraph responses. During that window, the UI cannot trust "legacy not found in subgraph" as ground truth: a legacy that was just created would be falsely reported as nonexistent.

The app handles this in two places:

- **Post-mutation freshness**: hooks like `useHasExistingLegacy` do a direct on-chain `readContract` call via viem with `blockTag: 'latest'` immediately after a mutation, so the UI state reflects the latest mined state without waiting for the subgraph.
- **Retry loop**: if an on-chain read returns an unexpected state (for example, the legacy still appears after a deletion), the hook retries a few times with short backoffs to let the node catch up.
- **A second source for "does this wallet have a legacy"**: the router keeps a flag per owner. Until 2026-11-18 a flaw cleared it on a repeat claim of a legacy that had already ended, even if the owner had created a new one since. No funds moved differently, and the pull vault refuses a second create while a live legacy is bound in it. Since 2026-11-11 the app confirms a "no legacy" answer against the vaults' live bindings before it offers a create. The router fix (only the claim that ends a legacy releases the flag) is live on mainnet since 2026-11-18 23:55 UTC; a flag cleared before it stays cleared, so the app keeps its check.

The net result: subgraphs give us speed, and on-chain reads give us correctness when speed isn't enough.

## Activity tracking

Solidity cannot directly read "the timestamp of `wallet.address`'s most recent outgoing transaction." The EVM doesn't expose it. So instead of trying to observe an owner's _general_ wallet activity, both legacy paths track an activity timestamp that the contracts can update natively; the difference is only in _what_ counts as activity.

### Safe owners: the Safe Guard

A smart-contract wallet can track activity internally. When the owner is a Safe, we install a **Safe Guard** (see the [Safe Guard integration](legacy-contracts-created-with-safe-sdk.md)) that records the timestamp of every Safe transaction. Any normal Safe activity (DeFi, staking, voting) implicitly keeps the legacy fresh, with no deliberate check-in. At activation time the Router reads that timestamp directly from the Guard and compares it against the configured inactivity window. Fully on-chain, no oracle.

### EOA owners: a per-legacy inactivity timer

A plain EOA has no on-chain code to hook into, so an EOA legacy tracks activity on the only surface it controls: **itself**. Each EOA legacy stores a `_lastTimestamp` (last-activity timestamp) and a `lackOfOutgoingTxRange` (the configured inactivity window). The eligibility check is a single on-chain comparison with no external call:

```solidity
// TransferEOALegacy._checkActiveLegacy()
if (_lastTimestamp + lackOfOutgoingTxRange > block.timestamp) {
  return false; // still considered active, activation not allowed
}
return true;     // inactive long enough, eligible
```

`_lastTimestamp` is reset to `block.timestamp` by any owner interaction with the legacy: creation, edits (beneficiaries / allocations / trigger), withdrawals, the auto-swap / unswap helpers, receiving ETH directly from the owner, or the explicit `activeAlive` ("I'm still alive") heartbeat, which a Premium owner can also sign and have relayed. On a legacy v3, setting NFT items, backup beneficiaries or the executor counts too. A beneficiary then activates permissionlessly via `TransferEOALegacyRouter.activeLegacy(legacyId, ...)`, and the contract itself decides whether the window has elapsed.

One addition in legacy v3 (wallet legacies created since 2026-11-13): if the owner named an executor, and that executor attests the owner's death after the owner has been silent for half the window, the date in the comparison above becomes 30 days after the attestation when that is sooner. Any owner interaction afterwards restores the normal date. It is still a pure on-chain comparison; what changes is that one named address can bring the date forward, and nobody verifies its attestation. See [An executor](legacy-contracts-created-with-eoas.md#an-executor).

The deliberate limitation: arbitrary transactions the owner makes _elsewhere_ are invisible to the per-legacy contract. By default, nothing bridges that information in: an EOA owner who is active on-chain but never touches their legacy will still see the timer count down, which is exactly why the heartbeat exists.

For owners who want their general wallet activity to count, there is one deliberate, opt-in exception: **auto-renew** (Premium). An attestor service observes the owner's public transaction count and calls the router to reset the timer when the wallet has sent a transaction since the countdown last restarted and the deadline is within 30 days. Every safety bound is on-chain (attestor-only entry point, opt-in required, strictly increasing nonce, a 30-day near-deadline window, and a 365-day budget since the owner's last real check-in), so a compromised attestor can only *delay* activation (bounded), never accelerate it or touch funds. Full mechanics and trust model: [EOA Activity & Auto-Renew](eoa-activity-auto-renew.md).

### Trust and verification

- **The contract is authoritative and self-contained.** Both paths reduce activation to an on-chain timestamp comparison. Nothing off-chain can fabricate a "yes, activate" signal or block a legitimate activation.
- **No party the owner did not name can trigger or accelerate activation.** There is no oracle network or third-party API in the activation path. The only off-chain writer anywhere near the timer is the opt-in [auto-renew attestor](eoa-activity-auto-renew.md), and it can only *reset* the timer (i.e. delay activation) within on-chain bounds; it has no power to make a legacy claimable earlier, and it never touches legacies that haven't opted in. The one address that can bring a claim date forward is an executor the owner chose on a legacy v3, within the limits above (half the window of silence first, then 30 days, cancelled by any check-in). 10102 holds no such role on any legacy.

### Failure modes

Because activation is purely on-chain, an outage of our app, backend, or the subgraph has **no effect on the ability to activate**: a beneficiary can call the router directly. The realistic EOA failure mode runs the other way: the owner forgets to check in and the timer elapses while they're still alive. That is recoverable (they heartbeat or edit to reset it), whereas a premature disbursement would not be. So by default the timer only ever resets on the owner's own deliberate, on-chain interactions, and even the opt-in auto-renew path can only err in the recoverable direction (a bounded delay, never an early activation).

The one feature that can err in the other direction is the optional executor of a legacy v3. A dishonest executor can bring the claim date forward once the owner has been silent for half the window, and the owner's own check-in within the 30 days is the only thing that stops it. An owner who names no executor is not exposed to this at all.

## What this means operationally

- Activation never waits on us. There is no activity oracle to be "down," and no subscription that can run dry and block a claim.
- The off-chain pieces that _do_ exist around the legacy (the [reminder worker](email-reminders.md), the [auto-renew attestor](eoa-activity-auto-renew.md), and the subgraph) are conveniences only, and all of them fail safe. If the attestor stops, renewals pause and reminder emails take over; if the worker or subgraph degrades, owners may miss a nudge or the UI may lag. Correctness and the ability to claim are unaffected in every case.
- Moving toward "every wallet is a smart wallet" (EIP-4337 account abstraction) would let EOA owners get the same passive, broad activity tracking Safe owners enjoy today; we expect to add an AA-specific integration once such wallets standardize a way to expose it.
