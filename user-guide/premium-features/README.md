---
description: >-
  Optional, time-bound capabilities on top of the free legacy features:
  backup beneficiary layers, watchers with an advisor view, email reminders,
  automatic renewal, and gas-free check-ins.
---

# Premium Features

Premium adds a second ring of protection around a plan that already works. The free tier is deliberately complete on its own: you can create, edit, activate, and claim legacies without ever subscribing. Premium is for owners who want the plan to keep taking care of itself: extra recovery layers, professional oversight, notifications, and automation that runs while you live your life.

## What Premium unlocks, at a glance

| Feature | What it does | Guide |
|---|---|---|
| **Contingent beneficiaries** | Up to two backup layers that become eligible if your primary beneficiaries never claim | [Manage Contingent Beneficiaries](./manage-contingent-beneficiaries.md) |
| **Authorized watchers + Advisor view** | Read-only oversight for a lawyer, advisor, or family member, with a consolidated dashboard when they watch several plans | [Manage Authorized Watchers](./manage-authorized-watchers.md) |
| **Email reminders** | Out-of-band nudges before deadlines, for you and your beneficiaries | [Configure Email Reminders](./configure-email-reminders.md) |
| **Automatic renewal** | Your normal wallet activity keeps your check-in timer fresh; no more remembering to check in | [Automatic Renewal](./automatic-renewal.md) |
| **Gas-free check-ins** | 10102 pays the network fee when you reset your timer | [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md) |

{% hint style="info" %}
**Beneficiaries never need Premium (or even ETH).** Gas-sponsored *claims* are free for everyone: if a beneficiary's wallet can't cover the network fee at claim time, the app asks for a free signature and 10102 pays the gas. See [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md).
{% endhint %}

## The Free baseline

Every user starts on the Free plan at no cost. The Free plan includes:

- Unlimited Legacy contracts and Timelocks.
- Unlimited beneficiaries.
- Auto-generated beneficiary keys with a printable backup (the Legacy Claim Card).
- Gas-sponsored claims for beneficiaries who hold no ETH.
- The Guardian AI assistant, with a free allowance for everyone.

On the Free plan you only ever pay Ethereum network gas. There is no subscription required for the core flows.

## Premium subscription

- **Model**: time-bound access, paid upfront, like an ENS name registration. You pick a duration, pay, and your Premium features are active for that window.
- **Accepted tokens**: ETH, USDC, or USDT.
- **On-chain record**: the subscription is recorded on-chain against your address.
- **Per-wallet**: subscriptions are scoped to the wallet that paid. If you manage multiple wallets, each needs its own subscription.

## What Premium unlocks

### Additional contingent beneficiary layers

Up to two additional fallback layers (a second-line and a third-line beneficiary) that become eligible to claim after configurable time windows if the primary beneficiaries don't act. Designed to handle the case where an intended beneficiary can't claim: lost keys, unreachable, deceased. See [Manage Contingent Beneficiaries](./manage-contingent-beneficiaries.md).

### Authorized watchers

Read-only accounts you authorize to view your legacies under **My Watchlist**. Useful for:

- Coordinating with an estate lawyer, where the lawyer needs to reference the plan without having access to your assets.
- Running a family dashboard, where one person keeps an eye on multiple setups.
- Pairing an off-chain will with an on-chain plan: the watcher sees enough to attest that the plan exists, without you exposing your wallet address.

Watchers have two visibility modes: **limited** (privacy-preserving; addresses are replaced with a unique identifier) or **full** (addresses visible). A watcher who follows several legacies, say a family advisor or estate attorney, gets a consolidated read-only **Advisor view** in the app: every contract they watch in one list, with status and check-in health at a glance. See [Manage Authorized Watchers](./manage-authorized-watchers.md).

### Email reminders

Opt-in email notifications for the owner and for each beneficiary, at configurable lead times before activation events. Sent for upcoming activations, timeline resets, actual activation, and successful claims. See [Configure Email Reminders](./configure-email-reminders.md).

### Automatic renewal

For wallet-based (EOA) legacies, an opt-in attestor watches your wallet's public on-chain activity and renews your check-in timer for you when you've been active near the deadline. Every safety bound is enforced by the contract, and the mechanism can only ever *delay* activation, never accelerate it. See [Automatic Renewal](./automatic-renewal.md).

### Gas-free check-ins

When you reset your inactivity timer, the app can ask for a free signature instead of a paid transaction, and 10102's relayer submits it and covers the network fee. Claims by beneficiaries are sponsored for everyone; check-ins are the Premium half of the same machinery. See [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md).

## Creator-only note for Safe legacies

For Safe-backed legacies, watchers and email reminders are managed by the **wallet that created the legacy** (the specific Safe signer who submitted the creation transaction), not by the Safe as a whole. Other Safe signers can still manage on-chain settings (beneficiaries, trigger) at the Safe's threshold, but they'll see a "Notification settings managed by 0x…abcd" badge in the watchers/reminders UIs.

If the creator needs to hand off notification control (for example, if they're leaving the Safe), the current workaround is to delete and recreate the legacy with the new signer as the submitter. See [Concepts: Creator vs. signer](../concepts.md#creator-vs-signer-safe-legacies).

## See also

- [Manage Contingent Beneficiaries](./manage-contingent-beneficiaries.md)
- [Manage Authorized Watchers](./manage-authorized-watchers.md)
- [Configure Email Reminders](./configure-email-reminders.md)
- [Automatic Renewal](./automatic-renewal.md)
- [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md)
