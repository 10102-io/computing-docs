---
description: >-
  Let your normal wallet activity keep your legacy fresh: an opt-in service
  renews your check-in countdown when you've been active on-chain, with every
  safety bound enforced by the contract.
---

# Automatic Renewal

Wallet-based (EOA) legacies work on a check-in timer. Checking in resets the countdown, and so does any change you make to the legacy. Sending, swapping or signing elsewhere does not, so checking in is up to you. If the countdown runs out, the legacy becomes claimable. That's the core promise, but it comes with a chore: remembering to check in, even during years when you never need to touch the plan.

**Automatic renewal** removes the chore. In the app's words: "Automatic renewal is on. If this wallet has sent a transaction since the countdown last restarted, we reset it for you in the last 30 days before your deadline. This works for up to 12 months after your last check-in and while Premium is active. Then check in yourself once and it carries on. You can check in yourself at any time." No click, no gas. You keep living on-chain; the plan keeps up.

## Turning it on

1. Open the legacy's details page and find the **Automatic renewal** switch.
2. Flip it on. A confirmation explains exactly what you're agreeing to, in the same words as this page.
3. Confirm the small transaction in your wallet. The setting is recorded on-chain, per legacy, and you can turn it off the same way at any time, Premium or not.

{% hint style="info" %}
**Works with check-in periods of about 13 months or less.** With a longer inactivity window, the renewal machinery could never fire before its own safety budget ran out (see below), so the switch tells you to shorten the window instead of pretending to protect you.
{% endhint %}

## What we watch, and what we can't see

We read one public number: your wallet's transaction count, the same figure anyone can see on Etherscan. There is no tracking beyond that: no browsing data, no location, no off-chain identity. If the count went up since your countdown last restarted, your wallet has sent a transaction, and in the last 30 days before your deadline the attestor renews your timer. Any transaction from the wallet counts, including one sent by an app or a bot you gave it to.

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

Nothing about the legacy itself changes in any of these cases. The timer keeps counting exactly as configured, and checking in yourself always works. [Home](../home.md) shows each legacy's timer with whether renewal can really run before the date.

Premium cannot be renewed early: renewal opens on its expiry date. If your Premium ends before the last 30 days of your countdown open, renewal will not run in time, so check in yourself before then.

## Emails and the Home notice

These go to the owner email saved in the legacy's [email reminders](./configure-email-reminders.md). Without one, the Home notice is your warning.

- **"Your legacy was renewed automatically."** Each time a renewal resets your countdown, you get this email instead of the ordinary "reset" one. It gives your next deadline and the date to check in yourself by to keep renewal working.
- **Premium ends soon.** 14 days before your Premium ends, an email and a notice on Home: "Your Premium ends on (date)." Renewal opens on that date; until then, check in to keep your timer safe.
- **Premium ended.** Once it has ended, an email and a notice on Home: "Automatic renewal is paused." Check in yourself before your deadline, or renew Premium and automatic renewal starts again by itself. Your setting stays on.

The Home notice always offers **Check in**, and **Renew Premium** once Premium has ended.

## Common questions

**Does this replace check-ins entirely?** Almost. You'll be asked to check in yourself once every 12 months at most, so that a real signal from you stays in the loop.

**Does it cost anything per renewal?** No. 10102 pays the gas for every automatic renewal. See [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md) for the other ways we cover fees.

**What about Safe legacies?** They don't need this: every Safe transaction already counts as activity automatically, via the Safe Guard.
