---
description: >-
  How opt-in auto-renew lets an EOA legacy follow your real wallet activity —
  an off-chain attestor observes your transaction count, and every safety
  bound is enforced on-chain, so the worst failure is a bounded delay.
---

# EOA Activity & Auto-Renew

An EOA legacy's inactivity countdown normally resets only when the owner
interacts with the legacy itself — an edit, a deposit, a withdrawal, or the
explicit heartbeat. Ordinary wallet activity (a send, a swap somewhere else
on-chain) is invisible to the legacy contract, because the EVM gives a
contract no way to read another account's transaction history. Active owners
still had to remember to check in.

**Auto-renew** closes that gap for owners who want it: an attestor service
watches your wallet's public transaction count (its nonce), and when it sees
you've been active near your deadline, it renews the timer for you — no
email, no click, no gas from you. It is a Premium feature, strictly opt-in
per legacy, and off by default.

This page explains how it works and — more importantly — exactly what the
attestor can and cannot do, because "a service watches your wallet" is a
claim that deserves scrutiny.

## The shape

- **The attestor is off-chain; the rules are on-chain.** A dedicated key
  held by our reminder service polls the owner's transaction count. When
  the count has risen and the legacy is near its activation deadline, the
  attestor calls `recordActivity(legacyId, observedNonce)` on the EOA
  legacy router. The router resets the legacy's inactivity timer exactly
  as an owner check-in would.
- **Router-only.** The mechanism lives in the upgradeable router, not in
  the per-legacy contracts — so it covers every existing EOA legacy with
  zero changes to deployed legacy contracts.
- **The attestor only ever attests.** It holds no funds, has no claim
  rights, and its only entry point is the timer reset described here.

## The on-chain bounds

Every safety property is enforced by the contract, not by the attestor
behaving well. A buggy or malicious attestor gets a loud revert, not a
silently shifted timeline.

| Bound | What it enforces |
|---|---|
| **Opt-in only** | You enable it per legacy with `setAutoRenew(legacyId, enabled)` — owner-only, Premium-gated, default off. Premium is re-checked at every renewal; if your subscription lapses, renewals pause and reminder emails take over. Disabling is always allowed, Premium or not. |
| **Strictly increasing nonce** | Each attestation must cite a higher transaction count than the last one recorded. An old observation can never be replayed — not even across disable/re-enable toggles. |
| **Near-deadline window** | Attestations only land within 30 days of the activation deadline, and never at or after it. That means roughly one renewal per inactivity period, and a legacy that has already become claimable can never be re-armed by an attestation — the claim window belongs to the beneficiaries, and only your own check-in can recover a lapsed legacy. |
| **365-day budget** | Renewals stop 365 days after your last *real* check-in. Attested renewals deliberately do not refill this budget — the attestor can't extend its own leash. When the budget runs out, reminder emails ask you to check in yourself, which refills it. |

Enabling also fails loudly when the feature could never fire — on a deleted
legacy, or when the configured inactivity window is so long (past roughly
395 days) that the renewal window would only ever open after the budget was
already spent.

## The trust model, honestly stated

A fully compromised attestor key can only **delay** activation — never
accelerate it, never claim, never move funds, never touch a legacy that
hasn't opted in, and never re-arm a legacy that is already claimable.

Stated precisely: the last renewal a compromised attestor can land is up to
12 months after your last real check-in, and activation then follows one
further full inactivity period. So the worst-case delay is **12 months plus
one inactivity period** past the natural deadline — about 15 months for a
90-day trigger. Bounded, and recoverable by you at any time.

Known edges, stated out loud:

- **Disable front-running.** An attestor watching the mempool could
  front-run your `setAutoRenew(id, false)` with one final renewal. That is
  bounded to a single period, and your disable still lands.
- **Nonce poisoning.** A malicious attestor could record an absurdly high
  nonce, permanently breaking auto-renew for that legacy. This fails
  *safe*: renewals stop, reminder emails take over, and activation proceeds
  on schedule. We accepted this rather than adding an owner-callable nonce
  reset, which would weaken the replay protection for a pure availability
  gain in an already-compromised scenario.
- **The kill switch.** The contract admin can rotate the attestor key or
  zero it entirely. Zeroing pauses all renewals system-wide without
  touching any owner's opt-in state. And because the router sits behind
  the timelocked upgrade process, the bounds themselves can't be quietly
  changed — see [Upgrade Policy](upgrade-policy.md).

The consent summary we show at opt-in is the same claim: *we watch this
wallet's public transaction count and renew for you when you're active;
after 12 months of renewals we ask you to check in yourself. If our systems
were ever compromised, the worst possible outcome is your beneficiaries'
claim being delayed — never redirected, never accelerated.*

## How it interacts with reminders

Auto-renew and [email reminders](email-reminders.md) are designed to hand
off to each other:

- If renewals stop for any reason — budget exhausted, Premium lapsed,
  attestor paused, or you raised your inactivity window past the
  feasibility limit — reminder emails take over and ask you to check in.
- An attested renewal is **not** treated as proof that you're reachable.
  Renewals emit a dedicated on-chain event distinct from a real check-in,
  and the reminders that ask you to "check in for real" as the budget runs
  down are never suppressed by attested renewals.

## Verify it yourself

- The EOA legacy router (mainnet proxy):
  [`0x4E81E1Ed3F6684EB948F8956b8787967b1a6275b`](https://etherscan.io/address/0x4E81E1Ed3F6684EB948F8956b8787967b1a6275b)
  — read `activityAttestor()` to see the current attestor (the worker key,
  `0x4B05aC1b0BF109A9CE30dCEc2831990d694d74D0`), and watch the
  `TransferEOALegacyAutoRenewed` event for every attested renewal.
- Every bound above is in the verified router source on Etherscan; nothing
  depends on the attestor's off-chain code being correct.
- Without opting in, nothing changes for you: your legacy's timer resets
  only on your own interactions with it, exactly as described in
  [Indexing & Activity Tracking](indexing-and-activity-tracking.md).
