---
description: >-
  Optional capabilities on top of the free legacy features, for 99 USD a
  year or 399 USD once: backup beneficiaries, backups for each heir and an
  executor, watchers with an advisor view, email reminders, automatic
  renewal, and gas-free check-ins and withdrawals. How to buy it, with a
  coupon code if you have one.
---

# Premium Features

Premium adds to a legacy that already works. The free tier is deliberately complete on its own: you can create, edit, activate, and claim legacies without ever subscribing. Premium is for owners who want more people in line when the first ones do not claim, oversight for an advisor, notifications, and automation that runs while you live your life.

**The price.** 99 USD per year, or 399 USD once for lifetime, paid on-chain in ETH, USDC or USDT. The plans are records in the Premium contract; the Pricing page in the app shows them as they are recorded there, and checkout reads the price again right before it charges.

## What Premium unlocks, at a glance

| Feature | What it does | Guide |
|---|---|---|
| **Backup beneficiary and second backup** | Up to two people, one after the other, who can claim once their wait has passed, for when your beneficiaries do not | [Backup Beneficiaries and Backup Heirs](./manage-contingent-beneficiaries.md) |
| **Backups for each heir** | On a wallet legacy of the new version: up to two backups for each heir's own share, each after a wait you choose | [Backup heirs](./manage-contingent-beneficiaries.md#backup-heirs-for-each-share) |
| **An executor** | On a wallet legacy of the new version: someone you trust who can confirm that you have died once you have been silent for half your check-in period, so your heirs can claim 30 days after the confirmation | [Name an Executor](./executor.md) |
| **Authorized watchers + Advisor view** | Read-only oversight for a lawyer, advisor, or family member, with a consolidated dashboard when they watch several legacies | [Manage Authorized Watchers](./manage-authorized-watchers.md) |
| **Email reminders** | Out-of-band nudges before deadlines, for you and your beneficiaries | [Configure Email Reminders](./configure-email-reminders.md) |
| **Automatic renewal** | Your normal wallet activity keeps your check-in timer fresh; no more remembering to check in | [Automatic Renewal](./automatic-renewal.md) |
| **Gas-free check-ins and withdrawals** | 10102 pays the network fee when you reset your timer, and when you withdraw your own timelock | [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md) |
| **Wallet alerts for more addresses** | Alerts for up to 20 other wallets and for Bitcoin addresses (your own wallet's alerts are free) | [Wallet alerts and the Bitcoin watch](../wallet-alerts.md) |
| **An ENS subname** | One free ENS subname each year, per parent name (for example `yourname.miami.eth`), as the Pricing page lists it | The Pricing page in the app |

{% hint style="info" %}
**Beneficiaries never need Premium (or even ETH).** Gas-sponsored *claims* are free for everyone: if a beneficiary's wallet can't pay the network fee at claim time, the app asks for a free signature and 10102 pays the gas. See [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md).
{% endhint %}

## The Free baseline

Every user starts on the Free plan at no cost: no subscription, ever. The Free plan includes:

- Legacy contracts (one for your wallet, one for each Safe you sign for) and as many Timelocks as you like (standard, soft and for someone else).
- Beneficiaries with shares, and auto-generated beneficiary keys with a printable backup sheet for people who have no wallet yet.
- NFTs in a wallet legacy of the new version, from the collections on the list.
- Shield Vault holdings: every withdrawal waits, and a recovery sheet can stop one you did not start if it is used before the wait ends.
- Check in, and update or delete a legacy until the first claim.
- Gas-sponsored claims for beneficiaries who hold no ETH.
- Wallet alerts for the wallet you are connected with.
- The Guardian AI assistant, with a free allowance for everyone.

On the Free plan you pay Ethereum network gas, and one fee from 10102: the fee when assets are delivered. It is 0.25% today, fixed into each legacy or Shield Vault holding when it is created and shown before you confirm. It is taken when your heirs claim tokens or ETH from a legacy created since 2026-09-29, and when a Shield Vault holding is released to the people you named after your silence. Setting one up and keeping it carry no fee, and neither do timelocks, gifts or NFTs handed on through a legacy. See [Create a Legacy Contract](../legacy/create-a-legacy-contract.md) and [Shield Vault](../shield.md).

## Premium subscription

- **Two plans**: one year for 99 USD, or lifetime for 399 USD once. Paid upfront; the yearly plan is time-bound, like an ENS name registration.
- **Accepted tokens**: ETH, USDC, or USDT. The USD price is converted at checkout, and the page shows the amount before you pay.
- **On-chain record**: the subscription is recorded on-chain against your address.
- **Per-wallet**: subscriptions are scoped to the wallet that paid. If you manage multiple wallets, each needs its own subscription.

## Buying Premium

Open **Settings**, **10102 Membership**. Pick a plan and a payment token, then **Purchase**.

- **Coupon code** (since 2026-11-09). Checkout has an optional **Coupon code** field, for a code a partner gave you. There is no public code. The page checks the code on-chain as you type and says what it does: a discount, a credit to the partner who sent you, or why it cannot be used (unknown, switched off, expired or used up). Clear the field to buy without it.
- **The price shown is the price charged.** Right before sending, the app reads the price again. With ETH, the transaction sends exactly that amount. With USDC or USDT, your wallet first approves exactly that amount, and an older, larger approval to the Premium contract is reduced to it. If the price changed in between, nothing is charged and the page shows the new price.
- **Renewal opens on the expiry date.** While Premium runs, it cannot be bought again, so it cannot be renewed early. Settings shows your expiry date; from that day, you can buy Premium again on the same page.

If you use [automatic renewal](./automatic-renewal.md), Premium ending matters: renewals stop with it. You are warned before and when it ends, by email and on Home; see [Automatic Renewal: emails and the Home notice](./automatic-renewal.md#emails-and-the-home-notice).

## What Premium unlocks

### A backup beneficiary and a second backup

Up to two fallback people for the whole legacy (the contracts call them the second line and the third line) who become eligible to claim after waits you set if your beneficiaries don't act. Designed to handle the case where an intended beneficiary can't claim: lost keys, unreachable, deceased. See [Backup Beneficiaries and Backup Heirs](./manage-contingent-beneficiaries.md).

### Backups for each heir, and an executor

On a wallet legacy of the new version (created since 2026-11-13 20:42 UTC, or [moved](../legacy/move-to-the-new-version.md) to it):

- **Backup heirs**: each heir can have up to two backups for their own share. If the heir has not claimed a wait after the claim date (you choose it, 30 to 3,650 days), their first backup can claim that share, and the second backup one more wait later. Each share is claimed on its own, so nobody waits for anyone else. See [Backup heirs](./manage-contingent-beneficiaries.md#backup-heirs-for-each-share).
- **An executor**: someone you trust, like a lawyer or a friend, who can confirm that you have died, once you have been silent for half your check-in period. Claims then open 30 days after the confirmation if that is sooner. Any check-in from you cancels it, and our server emails you about it only if this legacy has a reminder with your own email. Nobody verifies the confirmation. See [Name an Executor](./executor.md).

Both can be turned off without Premium; setting or changing them needs it. The new version is live without an independent audit; see [Legacy](../legacy/README.md#honest-limits-of-the-new-version).

### Authorized watchers

Read-only accounts you authorize to view your legacies under **My Watchlist**. Useful for:

- Coordinating with an estate lawyer, where the lawyer needs to reference the legacy without having access to your assets.
- Running a family dashboard, where one person keeps an eye on multiple setups.
- Pairing an off-chain will with an on-chain legacy: the watcher sees enough to attest that the legacy exists, without you exposing your wallet address.

Watchers have two visibility modes: **limited** (privacy-preserving; addresses are replaced with a unique identifier) or **full** (addresses visible). A watcher who follows several legacies, say a family advisor or estate attorney, gets a consolidated read-only **Advisor view** in the app: every contract they watch in one list, with status and check-in health at a glance. See [Manage Authorized Watchers](./manage-authorized-watchers.md).

### Email reminders

Opt-in email notifications for the owner and for each beneficiary, at configurable lead times before activation events. Sent for upcoming activations, timeline resets, actual activation, and successful claims. See [Configure Email Reminders](./configure-email-reminders.md).

### Automatic renewal

For wallet-based (EOA) legacies, an opt-in attestor watches your wallet's public on-chain activity. If your wallet has sent a transaction since the countdown last restarted, it resets the countdown for you in the last 30 days before your deadline. Every safety bound is enforced by the contract, and the mechanism can only ever *delay* activation, never accelerate it. See [Automatic Renewal](./automatic-renewal.md).

### Gas-free check-ins and withdrawals

When you check in on a wallet legacy or a Shield Vault holding, or withdraw one of your own timelocks once it opens, the app asks for a free signature instead of a paid transaction, and 10102's relayer submits it and pays the network fee. A withdrawal always goes to your own wallet. If the free send is not possible, your wallet sends a normal transaction instead. Claims by beneficiaries are sponsored for everyone; these are the Premium half of the same machinery. See [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md).

## Creator-only note for Safe legacies

For Safe-backed legacies, watchers and email reminders are managed by the **wallet that created the legacy** (the specific Safe signer who submitted the creation transaction), not by the Safe as a whole. Other Safe signers can still manage on-chain settings (beneficiaries, trigger) at the Safe's threshold, but they'll see a "Notification settings managed by 0x…abcd" badge in the watchers/reminders UIs.

If the creator needs to hand off notification control (for example, if they're leaving the Safe), the current workaround is to delete and recreate the legacy with the new signer as the submitter. See [Concepts: Creator vs. signer](../concepts.md#creator-vs-signer-safe-legacies).

## See also

- [Backup Beneficiaries and Backup Heirs](./manage-contingent-beneficiaries.md)
- [Name an Executor](./executor.md)
- [Manage Authorized Watchers](./manage-authorized-watchers.md)
- [Configure Email Reminders](./configure-email-reminders.md)
- [Automatic Renewal](./automatic-renewal.md)
- [Gas-Free Check-ins & Claims](./gas-free-check-ins-and-claims.md)
