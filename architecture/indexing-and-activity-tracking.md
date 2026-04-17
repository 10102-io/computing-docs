---
description: >-
  Subgraphs on The Graph keep the UI fast; Chainlink Functions + Moralis fill
  in the one thing the EVM can't tell us — an EOA's last outgoing transaction
  timestamp.
---

# Indexing & Activity Tracking

Two distinct problems sit under one banner here:

1. **Indexing** — reading structured data out of on-chain events quickly. Solved with [The Graph](https://thegraph.com).
2. **Activity tracking** — knowing when an EOA last sent a transaction. Solved with a [Chainlink](https://chain.link) + [Moralis](https://moralis.com) hybrid, because the EVM can't answer this natively.

Both layers are strictly additive: the on-chain contracts are authoritative, and the UI falls back to direct RPC reads when the indexed layer is stale or unavailable.

## Subgraphs on The Graph

We maintain multiple subgraphs per chain, each with a narrow responsibility. Splitting them keeps indexing time bounded and makes it easy to redeploy one without disturbing the others.

- **Legacy + timelock + reminders subgraph** — indexes every event emitted by the routers: `LegacyCreated`, `LegacyUpdated`, `LegacyDeleted`, `Activated`, `TimelockCreated`, `ReminderConfigured`, plus the Safe's own `ChangeGuard` / `EnableModule` / `AddedOwner` / `ChangedThreshold` for 10102-enabled Safes. The UI reads the bulk of its state from here.
- **Token balance subgraph** — indexes ERC-20, ERC-721, and ERC-1155 balances for wallets interacting with the app. Used to populate the "your assets" pickers during legacy creation.
- **ETH totals subgraph** — periodically aggregates native ETH held under each legacy contract and system-wide totals. Separated because ETH balances require a different indexing strategy than event-sourced state.

### Mainnet and Sepolia

Every subgraph is deployed on both mainnet and Sepolia. Deployment IDs and endpoint URLs are configuration; the app picks the right set based on the connected chain.

### Freshness and the on-chain fallback

Subgraphs are eventually-consistent. After a transaction is mined, it typically takes a handful of blocks for an event to appear in subgraph responses. During that window, the UI cannot trust "legacy not found in subgraph" as ground truth — a legacy that was just created would be falsely reported as nonexistent.

The app handles this in two places:

- **Post-mutation freshness** — hooks like `useHasExistingLegacy` do a direct on-chain `readContract` call via viem with `blockTag: 'latest'` immediately after a mutation, so the UI state reflects the latest mined state without waiting for the subgraph.
- **Retry loop** — if an on-chain read returns an unexpected state (for example, the legacy still appears after a deletion), the hook retries a few times with short backoffs to let the node catch up.

The net result: subgraphs give us speed, and on-chain reads give us correctness when speed isn't enough.

## Activity tracking for EOAs

This is the hard problem.

Solidity cannot directly read "the timestamp of `wallet.address`'s most recent outgoing transaction." The EVM doesn't expose it. A smart-contract wallet _can_ track it internally (see the [Safe Guard integration](legacy-contracts-created-with-safe-sdk.md)), but a plain EOA has no on-chain code to hook into.

For EOA-owned legacies, this is solved with a bridged off-chain check:

1. A beneficiary attempts to activate the legacy via `TransferEOALegacyRouter.activate(legacyId)`.
2. The Router invokes a **Chainlink Function**, passing the owner's EOA and the configured inactivity window.
3. The Chainlink Function — executed by multiple independent oracle nodes for redundancy — calls a **Moralis API** endpoint to retrieve the owner's transaction history.
4. The Function identifies the timestamp of the most recent outgoing transaction and returns it to the Router on-chain.
5. The Router compares the timestamp against `block.timestamp - configuredInactivityWindow`. If the owner has been inactive for long enough, the activation proceeds. If not, it reverts.

### Trust and verification

- **Chainlink's multi-node model** means no single oracle can lie unilaterally about the owner's activity. The result is only accepted if consensus is reached.
- **Moralis is a read-only dependency.** It can only read historical blockchain data that anyone with an RPC can read — it can't forge a transaction or hide one. In the worst case (Moralis returns wrong data), Chainlink's consensus threshold catches it because not all nodes would get the same wrong answer.
- **The smart contract remains authoritative.** Chainlink doesn't _grant_ activation; it supplies a timestamp, and the contract's `require(...)` logic decides whether that timestamp clears the threshold. There is no "Chainlink says activate" flow that skips the on-chain check.

### Failure modes

If Chainlink or Moralis are unavailable, activation for EOA legacies simply can't complete. The Router's Chainlink callback times out or the function fails, and the transaction reverts. This is the correct safety posture: **we never want to activate with stale or missing activity data**, because the consequence of a false-positive activation (premature disbursement) is much worse than the consequence of delay.

Safe-owned legacies are unaffected by this — their activity tracking is fully on-chain in the Safe Guard, so they don't depend on any oracle for activation.

## What this means operationally

- The UI will surface a clear error if a beneficiary tries to activate and the activity oracle is unavailable. The plan isn't broken; it's waiting.
- We run the Chainlink subscription ourselves and top it up well ahead of runway. Subscription exhaustion would look identical to oracle downtime from the user's perspective.
- Moving more of this on-chain is on the roadmap — EIP-4337 paymasters and on-chain activity oracles are the most promising directions. Until those are mature, the hybrid path is the honest trade-off for an EOA flow that doesn't require custody.
