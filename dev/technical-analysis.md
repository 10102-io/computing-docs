---
description: >-
  The core engineering challenge behind an on-chain inheritance product — and
  the trade-offs baked into how we solved it.
---

# Inactivity Detection

Every inheritance flow in this product turns on one question: _has the owner been inactive long enough to trigger activation?_ Getting that question right, **on-chain**, for an arbitrary Ethereum wallet, is harder than it sounds — and the shape of our product is largely a consequence of the trade-offs we accepted.

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
2. **Decentralized oracle (pure)** — delegate to a general-purpose oracle network that already tracks wallet activity. _Rejected (for now):_ no decentralized oracle we evaluated had production-grade coverage of EOA activity data. Chainlink + Moralis gets most of the way there but it's a hybrid, not a pure oracle.
3. **Custom wallet functionality** — require the owner to use a wallet that exposes a last-tx hook. _Partially adopted:_ this is exactly what the Safe Guard integration does for Safe users.
4. **Restructured flow** — redesign the UX so the contract doesn't need to know the owner's last-tx timestamp at all. _Considered, rejected:_ the alternatives we sketched (mandatory check-ins, lawyer-attested activation) either shift burden to the owner or reintroduce trust in a centralized party.

We ended up with a combination of (3) for Safe owners and a hybrid of (2) and (4) for EOA owners.

## The Safe path — approach 3

When the owner's wallet is a Safe (a smart-contract wallet with the plug-in points we need), we install a **Safe Guard**: a small contract Safe calls before every transaction execution. Our Guard updates `lastOutgoingTxTimestamp` on every call, storing it in its own contract state.

That's the entire trick. At activation time, our Router reads the timestamp directly from the Guard, compares it against `block.timestamp - configuredInactivityWindow`, and gates activation on the result. Fully on-chain, no oracle, no trust.

There's a subtle elegance here: the Guard works regardless of what the Safe is doing. A Safe owner's _normal_ transactions — DeFi, staking, voting, whatever — update the Guard implicitly. The owner doesn't need to remember to "check in." Normal usage _is_ the check-in.

This is why the Safe flow is the path we recommend for users who already have a Safe. It's cleaner, cheaper, and fully self-contained.

## The EOA path — hybrid of 2 and 4

EOAs have no code, so there's nothing to hook. We considered making the EOA flow mandatory-heartbeat ("log in every N days or your legacy activates early") but that's a terrible UX: it confuses "I want to protect my beneficiaries" with "I need to babysit an app."

The solution we settled on is a **Chainlink Functions + Moralis hybrid** at activation time, not continuously:

- When a beneficiary attempts to activate, the Router dispatches a Chainlink Functions request.
- The Function queries Moralis's transaction-history API for the owner's EOA.
- Multiple Chainlink oracle nodes run the query independently and reach consensus on the result.
- The consensus timestamp is returned on-chain; the Router's `require(...)` logic decides whether it clears the inactivity threshold.

The important property: **Chainlink and Moralis don't _grant_ activation — they supply a data point, and the smart contract decides**. No oracle can fabricate a "yes, activate" signal that bypasses the contract's own check. The worst an oracle can do is return a wrong timestamp; consensus across multiple nodes makes that very hard.

### Why not apply this to Safes too?

For uniformity, we could skip the Safe Guard entirely and use the Chainlink path for everyone. We don't, for two reasons:

- **Purity** — the Safe Guard is fully on-chain, with no external dependency. For Safe users, we can deliver the strongest guarantee (no oracle in the trust path). We'd rather let Safe users have that cleaner property than force them down to EOA parity.
- **Cost** — every Chainlink Functions call burns LINK. For Safes, the on-chain Guard is free at activation time.

## Failure modes and their consequences

| Failure | Safe legacy | EOA legacy |
|---|---|---|
| Oracle / Moralis outage | No impact — fully on-chain | Activation blocked until oracle returns. Plan pauses, doesn't break. |
| Owner changes signer set on Safe | Guard still tracks activity; legacy continues to work. | N/A |
| Owner rotates EOA key | Key rotation doesn't show as "outgoing transaction" unless they move funds — so the inactivity timer _keeps counting_. Owners need to trigger an explicit heartbeat or an edit after rotation. | |
| Guard gets disabled without a matching legacy delete | Edge case we test against. Router detects missing Guard at activation and blocks. Legacy can be cleaned up via the explicit delete flow. | N/A |

The failure we care about most is the one that activates _too early_ — because it's irreversible. Every other failure mode (too late, blocked, paused) can be recovered with manual intervention. Our design explicitly prefers "pause on doubt" over "proceed with stale data."

## Future: account abstraction

EIP-4337 and the broader account-abstraction push make this problem easier. An AA wallet can expose its own `lastOutgoingTxTimestamp` as naturally as a Safe does today, and eventually "every wallet is a smart wallet" collapses the EOA/Safe distinction entirely. We expect to add an AA-specific integration when AA wallets have standardized a reasonable way to expose this.

Until then, the two-path design is the best honest trade-off we've found.
