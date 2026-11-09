---
description: >-
  How opt-in auto-renew lets an EOA legacy follow your real wallet activity:
  an off-chain attestor observes your transaction count, and every safety
  bound is enforced on-chain, so the worst failure is a bounded delay.
---

# EOA Activity & Auto-Renew

An EOA legacy's inactivity countdown normally resets only when the owner
checks in or changes the legacy itself. Ordinary wallet activity (a send, a
swap somewhere else on-chain) is invisible to the legacy contract, because
the EVM gives a contract no way to read another account's transaction
history. Active owners still had to remember to check in.

**Auto-renew** closes that gap for owners who want it: an attestor service
watches your wallet's public transaction count (its nonce). If your wallet
has sent a transaction since the countdown last restarted, it renews the
timer for you in the last 30 days before your deadline: no click, no gas
from you. It is a Premium feature, strictly opt-in per legacy, and off by
default.

This page explains how it works and, more importantly, exactly what the
attestor can and cannot do, because "a service watches your wallet" is a
claim that deserves scrutiny.

## The shape

- **The attestor is off-chain; the rules are on-chain.** A dedicated key
  held by our reminder service polls the owner's transaction count. When
  the wallet has sent a transaction since the countdown last restarted and
  the legacy is within 30 days of its activation deadline, the attestor
  calls `recordActivity(legacyId, observedNonce)` on the EOA legacy router.
  The router resets the legacy's inactivity timer exactly as an owner
  check-in would.
- **Activity must come after the last restart (since 2026-11-02).** The
  attestor reads the owner's transaction count at the block where the
  countdown last restarted (a check-in, a change to the legacy, a renewal,
  or the create itself) and renews only when today's count is higher, and
  higher than the last count it cited. Before, it compared only with its
  own last observation, so the owner's own check-in or opt-in counted as
  new activity: an owner who checked in and then died still got one
  renewal. If the historical read is unavailable, the attestor skips the
  legacy, which is the safe direction. Renewals of one legacy are also at
  least a day apart (or half the inactivity period, if that is shorter).
  These are the attestor's own rules: the contract cannot read an
  account's nonce, so it enforces the bounds below instead.
- **Router-only.** The mechanism lives in the upgradeable router, not in
  the per-legacy contracts, so it covers every existing EOA legacy with
  zero changes to deployed legacy contracts.
- **The attestor only ever attests.** It holds no funds, has no claim
  rights, and its only entry point is the timer reset described here.

## The on-chain bounds

Every safety property is enforced by the contract, not by the attestor
behaving well. A buggy or malicious attestor gets a loud revert, not a
silently shifted timeline.

| Bound | What it enforces |
|---|---|
| **Opt-in only** | You enable it per legacy with `setAutoRenew(legacyId, enabled)`: owner-only, Premium-gated, default off. Premium is re-checked at every renewal; if your subscription lapses, renewals pause and reminder emails take over. Disabling is always allowed, Premium or not. |
| **Strictly increasing nonce** | Each attestation must cite a higher transaction count than the last one that attestor recorded for the legacy (`attestorNonceSeen`, per attestor since the router upgrade of 2026-11-07). An old observation can never be replayed, not even across disable/re-enable toggles. |
| **Near-deadline window** | Attestations only land within 30 days of the activation deadline, and never at or after it. That means roughly one renewal per inactivity period, and a legacy that has already become claimable can never be re-armed by an attestation: the claim window belongs to the beneficiaries, and only your own check-in can recover a lapsed legacy. |
| **365-day budget** | Renewals stop 365 days after your last *real* check-in. Attested renewals deliberately do not refill this budget: the attestor can't extend its own leash. When the budget runs out, reminder emails ask you to check in yourself, which refills it. |

Enabling also fails loudly when the feature could never fire: on a deleted
legacy, or when the configured inactivity window is so long (past roughly
395 days) that the renewal window would only ever open after the budget was
already spent.

## The trust model, honestly stated

A fully compromised attestor key can only **delay** activation: never
accelerate it, never claim, never move funds, never touch a legacy that
hasn't opted in, and never re-arm a legacy that is already claimable.

Stated precisely: the last renewal a compromised attestor can land is up to
12 months after your last real check-in, and activation then follows one
further full inactivity period. So the worst-case delay is **12 months plus
one inactivity period** past the natural deadline, about 15 months for a
90-day trigger. Bounded, and recoverable by you at any time.

Known edges, stated out loud:

- **Disable front-running.** An attestor watching the mempool could
  front-run your `setAutoRenew(id, false)` with one final renewal. That is
  bounded to a single period, and your disable still lands.
- **Nonce poisoning.** A malicious attestor could record an absurdly high
  nonce, breaking auto-renew for that legacy while that attestor stays in
  place. This fails *safe*: renewals stop, reminder emails take over, and
  activation proceeds on schedule. Since 2026-11-07 the bound is kept per
  attestor, so rotating to a new attestor key restores renewals. We
  accepted this rather than adding an owner-callable nonce reset, which
  would weaken the replay protection for a pure availability gain in an
  already-compromised scenario.
- **The kill switch.** The governance Safe can zero the attestor at once
  (`setActivityAttestor(0)`), which pauses all renewals system-wide without
  touching any owner's opt-in state. Pointing it at a new key waits in the
  public 48-hour upgrade timelock. And because the router sits behind the
  timelocked upgrade process, the bounds themselves can't be quietly
  changed; see [Upgrade Policy](upgrade-policy.md).

The consent summary we show at opt-in is the same claim: *we watch this
wallet's public transaction count and renew for you when you're active;
after 12 months of renewals we ask you to check in yourself. If our systems
were ever compromised, the worst possible outcome is your beneficiaries'
claim being delayed, never redirected, never accelerated.*

## How it interacts with reminders

Auto-renew and [email reminders](email-reminders.md) are designed to hand
off to each other:

- If renewals stop for any reason (budget exhausted, Premium lapsed,
  attestor paused, or you raised your inactivity window past the
  feasibility limit), reminder emails take over and ask you to check in.
- An attested renewal is **not** treated as proof that you're reachable.
  Renewals emit a dedicated on-chain event distinct from a real check-in,
  and the reminders that ask you to "check in for real" as the budget runs
  down are never suppressed by attested renewals.

## Verify it yourself

- The EOA legacy router (mainnet proxy):
  [`0x4E81E1Ed3F6684EB948F8956b8787967b1a6275b`](https://etherscan.io/address/0x4E81E1Ed3F6684EB948F8956b8787967b1a6275b).
  Read `activityAttestor()` to see the current attestor (the worker key,
  `0x0925B872a4F4a39544A5a191dB67C42E30240a1E` since its rotation on
  2026-10-22), and watch the
  `TransferEOALegacyAutoRenewed` event for every attested renewal.
- Every bound above is in the verified router source on Etherscan; nothing
  depends on the attestor's off-chain code being correct.
- Without opting in, nothing changes for you: your legacy's timer resets
  only on your own interactions with it, exactly as described in
  [Indexing & Activity Tracking](indexing-and-activity-tracking.md).
