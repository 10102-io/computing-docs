---
description: >-
  The core engineering challenge behind an on-chain legacy product — and
  the trade-offs baked into how we solved it.
---

# Inactivity Detection

Every legacy flow in this product turns on one question: _has the owner been inactive long enough to trigger activation?_ Getting that question right, **on-chain**, for an arbitrary Ethereum wallet, is harder than it sounds — and the shape of our product is largely a consequence of the trade-offs we accepted.

## The core problem

A smart contract executing on the EVM has very limited information about other accounts:

- It can read state of _contract_ accounts it's given the address of.
- It can read its own past transactions.
- It **cannot** directly read the timestamp of the most recent outgoing transaction from an externally-owned account (EOA).

The last point is the constraint. There's no `block.getLastTxOf(wallet)` or equivalent. You can read _what_ happened to an account's storage, but an EOA has no storage — just a balance and a nonce, neither of which timestamps anything.

This is a deliberate EVM design choice (the state trie doesn't carry per-account metadata like last-tx-time), not an oversight. Working around it is the job of the application layer.

## Four possible approaches

When we started, four approaches were on the table:

1. **Off-chain oracle** — have a backend service watch wallets and submit activity attestations on demand. _Rejected:_ centralizes trust on our backend, violates "plan survives us."
2. **Decentralized oracle** — delegate to a general-purpose oracle network that already tracks wallet activity. _Rejected:_ no decentralized oracle we evaluated had production-grade coverage of EOA activity data, and bridging a third-party activity verdict back on-chain at activation time adds an external dependency (and a recurring cost) to the one moment that matters most. We'd rather not put an oracle in the activation trust path.
3. **Custom wallet functionality** — require the owner to use a wallet that exposes a last-tx hook. _Partially adopted:_ this is exactly what the Safe Guard integration does for Safe users.
4. **Restructured flow** — redesign the UX so the contract doesn't need to know the owner's _general_ last-tx timestamp at all, and instead tracks activity that the contract _can_ see natively. _Adopted for EOAs:_ each EOA legacy keeps an on-chain inactivity timer scoped to the legacy itself, reset by any interaction with it (edits, withdrawals, swaps) or an explicit "I'm still alive" heartbeat.

We ended up with (3) for Safe owners and (4) for EOA owners.

## The Safe path — approach 3

When the owner's wallet is a Safe (a smart-contract wallet with the plug-in points we need), we install a **Safe Guard**: a small contract (`SafeGuard`) that Safe calls before every transaction execution via the Guard interface's `checkTransaction` hook. Our Guard stamps `lastTimestampTxs = block.timestamp` on every such call, storing it in its own contract state.

That's the entire trick. At activation time, the per-legacy contract's on-chain check reads that timestamp directly from the Guard (`getLastTimestampTxs()`) and gates activation on `lastTimestampTxs + inactivityWindow <= block.timestamp`. Fully on-chain, no oracle, no trust.

There's a subtle elegance here: the Guard works regardless of what the Safe is doing. A Safe owner's _normal_ transactions — DeFi, staking, voting, whatever — update the Guard implicitly. The owner doesn't need to remember to "check in." Normal usage _is_ the check-in.

This is why the Safe flow is the path we recommend for users who already have a Safe. It's cleaner, cheaper, and fully self-contained.

## The EOA path — an on-chain inactivity timer

EOAs have no code, so there's nothing to hook and no way for a contract to observe what the owner does _elsewhere_ on-chain. Rather than reach for an external oracle to fill that gap, the EOA path narrows the question to something the contract _can_ answer by itself: **has the owner interacted with this legacy recently?**

Each EOA legacy stores its own `_lastTimestamp` (a last-activity timestamp) and a `lackOfOutgoingTxRange` (the configured inactivity window). The check at activation time is a single on-chain comparison, with no external call:

```solidity
// TransferEOALegacy._checkActiveLegacy()
if (_lastTimestamp + lackOfOutgoingTxRange > block.timestamp) {
  return false; // owner still considered active — activation not allowed
}
return true;     // inactive long enough — eligible for activation
```

`_lastTimestamp` is reset to `block.timestamp` by any owner interaction with the legacy: creating it, editing beneficiaries/allocations/trigger/name, withdrawing, the auto-swap / unswap helpers, receiving ETH directly from the owner, or the explicit `activeAlive` ("I'm still alive") heartbeat. A beneficiary can then call activation permissionlessly; the contract itself decides whether the window has elapsed.

This is, in effect, the check-in model — the thing teams usually reject as "babysit the app" UX. We make it tolerable two ways: **normal management of the legacy already counts** (any edit resets the timer, so an owner who is actively maintaining their plan rarely needs a bare heartbeat), and **the heartbeat is cheap and explicit** (~21k gas, one button) for owners who aren't otherwise touching it.

The honest trade-off: an EOA owner who is busy on-chain elsewhere — but never touches their legacy — will see the timer keep counting. Arbitrary transactions from their EOA are invisible to the per-legacy contract. There is **no oracle filling that gap**; the owner is expected to check in. We accept that limitation in exchange for an activation path that is fully on-chain, free of external dependencies, and impossible for any off-chain party to spoof.

### Why the Safe path looks different

Safe owners get a strictly nicer property: the Safe Guard updates the activity timestamp on _every_ Safe transaction, so a Safe owner's normal on-chain activity (DeFi, staking, voting) implicitly keeps the legacy fresh — no deliberate check-in required. EOA owners only get credit for activity that touches the legacy. Both paths are fully on-chain at activation time; the Safe path simply observes a broader definition of "active" because the wallet itself cooperates.

## Failure modes and their consequences

| Failure | Safe legacy | EOA legacy |
|---|---|---|
| App / backend / indexer outage | No impact on activation — the on-chain comparison runs without any of them. | No impact on activation — same on-chain comparison, no oracle in the path. |
| Owner active elsewhere but never touches the legacy | Guard captures the activity automatically; timer stays fresh. | Timer _keeps counting_ — only legacy interactions count. Owner must heartbeat or edit to reset it. |
| Owner changes signer set on Safe | Guard still tracks activity; legacy continues to work. | N/A |
| Owner rotates EOA key | N/A | Key rotation isn't visible to the legacy at all; the inactivity timer keeps counting. Owners need an explicit heartbeat or edit after rotation. |
| Guard gets disabled without a matching legacy delete | Edge case we test against. Router detects missing Guard at activation and blocks. Legacy can be cleaned up via the explicit delete flow. | N/A |

The failure we care about most is the one that activates _too early_ — because it's irreversible. For EOAs the relevant risk runs the other way: the timer can only be reset by the owner, so the realistic failure is activating _too late_ (owner forgot to check in), which is recoverable — they just heartbeat. Our design prefers a recoverable delay over an irreversible premature disbursement.

## Future: account abstraction

EIP-4337 and the broader account-abstraction push make this problem easier. An AA wallet can expose its own last-transaction timestamp as naturally as a Safe's Guard does today, and eventually "every wallet is a smart wallet" collapses the EOA/Safe distinction entirely. We expect to add an AA-specific integration when AA wallets have standardized a reasonable way to expose this.

Until then, the two-path design is the best honest trade-off we've found.
