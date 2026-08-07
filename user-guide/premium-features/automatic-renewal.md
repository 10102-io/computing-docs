---
description: >-
  Let your normal wallet activity keep your legacy fresh: an opt-in service
  renews your check-in countdown when you've been active on-chain, with every
  safety bound enforced by the contract.
---

# Automatic Renewal

Wallet-based (EOA) legacies work on a check-in timer: if you don't interact with your legacy for the inactivity window you configured, it becomes claimable. That's the core promise, but it comes with a chore: remembering to check in, even during years when you never need to touch the plan.

**Automatic renewal** removes the chore. When it's on, 10102 watches your wallet's *public* on-chain activity (its transaction count). If you've been active close to your deadline, we reset the countdown for you: no click, no email, no gas. You keep living on-chain; the plan keeps up.

## Turning it on

1. Open the legacy's details page and find the **Automatic renewal** switch.
2. Flip it on. A confirmation explains exactly what you're agreeing to, in the same words as this page.
3. Confirm the small transaction in your wallet. The setting is recorded on-chain, per legacy, and you can turn it off the same way at any time, Premium or not.

{% hint style="info" %}
**Works with check-in periods of about 13 months or less.** With a longer inactivity window, the renewal machinery could never fire before its own safety budget ran out (see below), so the switch tells you to shorten the window instead of pretending to protect you.
{% endhint %}

## What we watch, and what we can't see

We read one public number: your wallet's transaction count, the same figure anyone can see on Etherscan. There is no tracking beyond that: no browsing data, no location, no off-chain identity. If the count went up near your deadline, you've been active, and the attestor renews your timer.

## The safety rails, in plain words

Every rule below is enforced by the smart contract, not by us promising to behave:

- **Only you can turn it on or off**, per legacy, and it's off by default.
- **Renewals only happen near your deadline** (within the last 30 days of the countdown), and never after a legacy has already become claimable. A lapsed legacy belongs to your beneficiaries; only your own check-in can recover it.
- **Automatic renewals stop 12 months after your last real check-in.** When the budget runs out, reminder emails ask you to check in yourself once, which refills it. The service cannot extend its own leash.
- **The worst possible failure is a delay.** If our monitoring were ever wrong or compromised, the only possible effect is your beneficiaries waiting longer, which is bounded and recoverable by you at any time. Renewals can never speed up activation, redirect funds, or touch your contract in any other way.

The full trust model, including exactly what a compromised attestor key could and couldn't do, is in [EOA Activity & Auto-Renew](../../architecture/eoa-activity-auto-renew.md).

## When renewals pause

Renewals stop (and email reminders take over) whenever:

- the 12-month budget since your last real check-in runs out,
- your Premium subscription lapses (your opt-in is remembered; renewals resume if you re-subscribe),
- or you turn the switch off.

Nothing about the legacy itself changes in any of these cases. The timer keeps counting exactly as configured, and checking in yourself always works.

## Common questions

**Does this replace check-ins entirely?** Almost. You'll be asked to check in yourself once every 12 months at most, so that a real signal from you stays in the loop.

**Does it cost anything per renewal?** No. 10102 pays the gas for every automatic renewal. See [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md) for the other ways we cover fees.

**What about Safe legacies?** They don't need this: every Safe transaction already counts as activity automatically, via the Safe Guard.
