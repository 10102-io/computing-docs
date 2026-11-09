---
description: >-
  Optional, time-bound capabilities on top of the free legacy features:
  backup beneficiary layers, watchers with an advisor view, email reminders,
  automatic renewal, and gas-free check-ins and withdrawals. How to buy it,
  with a partner code if you have one.
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
| **Gas-free check-ins and withdrawals** | 10102 pays the network fee when you reset your timer, and when you withdraw your own timelock | [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md) |
| **Wallet alerts for more addresses** | Alerts for up to 20 other wallets and for Bitcoin addresses (your own wallet's alerts are free) | [Wallet alerts and the Bitcoin watch](../wallet-alerts.md) |

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

## Buying Premium

Open **Settings**, **10102 Membership**. Pick a plan and a payment token, then **Purchase**.

- **Coupon code.** Checkout has an optional **Coupon code** field, for a code a partner gave you. There is no public code. The page checks the code on-chain as you type and says what it does: a discount, a credit to the partner who sent you, or why it cannot be used (unknown, switched off, expired or used up). Clear the field to buy without it.
- **The price shown is the price charged.** Right before sending, the app reads the price again. With ETH, the transaction sends exactly that amount. With USDC or USDT, your wallet first approves exactly that amount, and an older, larger approval to the Premium contract is reduced to it. If the price changed in between, nothing is charged and the page shows the new price.
- **Renewal opens on the expiry date.** While Premium runs, it cannot be bought again, so it cannot be renewed early. Settings shows your expiry date; from that day, you can buy Premium again on the same page.

If you use [automatic renewal](./automatic-renewal.md), Premium ending matters: renewals stop with it. You are warned before and when it ends, by email and on Home; see [Automatic Renewal: emails and the Home notice](./automatic-renewal.md#emails-and-the-home-notice).

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

For wallet-based (EOA) legacies, an opt-in attestor watches your wallet's public on-chain activity. If your wallet has sent a transaction since the countdown last restarted, it resets the countdown for you in the last 30 days before your deadline. Every safety bound is enforced by the contract, and the mechanism can only ever *delay* activation, never accelerate it. See [Automatic Renewal](./automatic-renewal.md).

### Gas-free check-ins and withdrawals

When you check in on a wallet legacy or a Shield Vault holding, or withdraw one of your own timelocks once it opens, the app asks for a free signature instead of a paid transaction, and 10102's relayer submits it and covers the network fee. A withdrawal always goes to your own wallet. If the free send is not possible, your wallet sends a normal transaction instead. Claims by beneficiaries are sponsored for everyone; these are the Premium half of the same machinery. See [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md).

## Creator-only note for Safe legacies

For Safe-backed legacies, watchers and email reminders are managed by the **wallet that created the legacy** (the specific Safe signer who submitted the creation transaction), not by the Safe as a whole. Other Safe signers can still manage on-chain settings (beneficiaries, trigger) at the Safe's threshold, but they'll see a "Notification settings managed by 0x…abcd" badge in the watchers/reminders UIs.

If the creator needs to hand off notification control (for example, if they're leaving the Safe), the current workaround is to delete and recreate the legacy with the new signer as the submitter. See [Concepts: Creator vs. signer](../concepts.md#creator-vs-signer-safe-legacies).

## See also

- [Manage Contingent Beneficiaries](./manage-contingent-beneficiaries.md)
- [Manage Authorized Watchers](./manage-authorized-watchers.md)
- [Configure Email Reminders](./configure-email-reminders.md)
- [Automatic Renewal](./automatic-renewal.md)
- [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md)
