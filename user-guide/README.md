---
description: >-
  How to use 10102 Computing Legacy, from connecting your wallet to creating
  legacies, timelocks, and managing them over time.
---

# User Guide

This guide covers every user-facing flow in the app. Pages are ordered roughly the way a new user would encounter them. Start at the top if this is your first visit, or jump to whatever section matches what you're trying to do.

Unfamiliar with the vocabulary? Skim [Concepts](./concepts.md) first.

## Sections

### Getting started

- [Authentication](./authentication.md): connecting and disconnecting a wallet, and signing in on each device.
- [Concepts](./concepts.md): short glossary of the terms used throughout this guide.
- [Home: your plan and your locks](./home.md): facts about your plans read from the chain, and your timelocks with their opening dates.
- [Quick Actions](./quick-actions.md): shortcuts on the Legacy and Timelock pages that pre-fill common setups.

### Legacy

- [Legacy](./legacy/README.md): overview of Transfer vs. Multisig legacies.
- [Create a Legacy Contract](./legacy/create-a-legacy-contract.md).
- [Legacy Contract Details](./legacy/legacy-contract-details.md).
- [Edit or Delete a Legacy Contract](./legacy/edit-or-delete-a-legacy-contract.md).
- [Activate a Legacy Contract and Claim Funds](./legacy/activate-a-legacy-contract-and-claim-funds.md): the beneficiary's side.
- [Legacy Claim Card](./legacy/legacy-claim-card.md): the printable safety net.

### Premium features

- [Premium Features](./premium-features/README.md): subscription model and what it unlocks.
- [Manage Contingent Beneficiaries](./premium-features/manage-contingent-beneficiaries.md).
- [Manage Authorized Watchers](./premium-features/manage-authorized-watchers.md).
- [Configure Email Reminders](./premium-features/configure-email-reminders.md).
- [Automatic Renewal](./premium-features/automatic-renewal.md): when your wallet has sent a transaction, we reset your legacy's countdown in the last 30 days before its deadline.
- [Gas-Free Check-ins & Claims](./premium-features/gas-free-check-ins-and-claims.md): free claims for every beneficiary, free check-ins and timelock withdrawals for Premium owners.

### Timelock

- [Timelock](./timelock/README.md): overview of Timelock, Soft Timelock, Beneficiary Timelock.
- [Heirloom (sealed gifts)](./heirloom.md): seal ETH or USDC as a gift that opens on a chosen date.
- [Using Timelock](./timelock/using-timelock.md): create, unlock, and claim for all three flavors.
- [Save (a monthly lock)](./save.md): put a little aside, locked until a date you choose.

### Protecting what you hold

- [Shield Vault (protect a holding)](./shield.md): a wait on every withdrawal, and a recovery sheet that stops a stolen key.
- [Wallet alerts and the Bitcoin watch](./wallet-alerts.md): an email when a wallet moves, free for your own; Bitcoin addresses in your watch list.

## A note on philosophy

10102 is deliberately built so that your plan survives us. Every feature in this guide can be operated directly from the Ethereum contracts, without our UI. That's why [Legacy Claim Card](./legacy/legacy-claim-card.md) exists, and why [Architecture](../architecture/README.md) is published alongside the user guide. If something about our UI is confusing or our servers are down, the plan doesn't stop working. That's the point.
