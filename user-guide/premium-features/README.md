---
description: >-
  Optional, time-bound capabilities on top of the free legacy features —
  contingent beneficiaries, authorized watchers, and email reminders.
---

# Premium Features

Premium unlocks three families of capabilities that complement the free legacy flow. The free tier is deliberately complete on its own: you can create, edit, activate, and claim legacies without ever subscribing. Premium is for owners who want additional recovery layers, oversight, or notifications.

## Premium subscription

- **Model**: time-bound access, paid upfront, like an ENS name registration. You pick a duration, pay, and your Premium features are active for that window.
- **Accepted tokens**: ETH, USDC, or USDT.
- **On-chain record**: the subscription is recorded on-chain against your address.
- **Per-wallet**: subscriptions are scoped to the wallet that paid. If you manage multiple wallets, each needs its own subscription.

## What Premium unlocks

### Additional contingent beneficiary layers

Up to two additional fallback layers — a second-line and a third-line beneficiary — that become eligible to claim after configurable time windows if the primary beneficiaries don't act. Designed to handle the case where an intended beneficiary can't claim (lost keys, unreachable, deceased). See [Manage Contingent Beneficiaries](./manage-contingent-beneficiaries.md).

### Authorized watchers

Read-only accounts you authorize to view your legacies under **My Watchlist**. Useful for:

- Coordinating with an estate lawyer, where the lawyer needs to reference the plan without having access to your assets.
- Running a family dashboard, where one person keeps an eye on multiple setups.
- Pairing an off-chain will with an on-chain plan — the watcher sees enough to attest that the plan exists, without you exposing your wallet address.

Watchers have two visibility modes: **limited** (privacy-preserving; addresses are replaced with a unique identifier) or **full** (addresses visible). See [Manage Authorized Watchers](./manage-authorized-watchers.md).

### Email reminders

Opt-in email notifications for the owner and for each beneficiary, at configurable lead times before activation events. Sent for upcoming activations, timeline resets, actual activation, and successful claims. See [Configure Email Reminders](./configure-email-reminders.md).

## Creator-only note for Safe legacies

For Safe-backed legacies, watchers and email reminders are managed by the **wallet that created the legacy** (the specific Safe signer who submitted the creation transaction), not by the Safe as a whole. Other Safe signers can still manage on-chain settings (beneficiaries, trigger) at the Safe's threshold, but they'll see a "Notification settings managed by 0x…abcd" badge in the watchers/reminders UIs.

If the creator needs to hand off notification control — for example, if they're leaving the Safe — the current workaround is to delete and recreate the legacy with the new signer as the submitter. See [Concepts — Creator vs. signer](../concepts.md#creator-vs-signer-safe-legacies).

## See also

- [Manage Contingent Beneficiaries](./manage-contingent-beneficiaries.md)
- [Manage Authorized Watchers](./manage-authorized-watchers.md)
- [Configure Email Reminders](./configure-email-reminders.md)
