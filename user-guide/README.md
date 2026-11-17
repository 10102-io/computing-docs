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
- [Home: your setup and your locks](./home.md): what is in your legacy, facts about your setup read from the chain, and your timelocks with their opening dates.
- [Hide amounts and Cypherpunk Mode](./hide-amounts-and-cypherpunk-mode.md): the two switches in the header, one for sharing your screen, one for using the app with fewer services.
- [Quick Actions](./quick-actions.md): shortcuts on the Legacy and Timelock pages that pre-fill common setups.

### Legacy

- [Legacy](./legacy/README.md): overview of Transfer vs. Multisig legacies, and the new version of the wallet legacy.
- [Create a Legacy Contract](./legacy/create-a-legacy-contract.md).
- [Legacy Contract Details](./legacy/legacy-contract-details.md).
- [Edit or Delete a Legacy Contract](./legacy/edit-or-delete-a-legacy-contract.md).
- [NFTs in a Legacy](./legacy/nfts-in-a-legacy.md): name single NFTs for an heir, from the collections on the list.
- [Move a Legacy to the New Version](./legacy/move-to-the-new-version.md): what the move keeps and what it changes.
- [Activate a Legacy Contract and Claim Funds](./legacy/activate-a-legacy-contract-and-claim-funds.md): the beneficiary's side, with the heir page.
- [Legacy Claim Card](./legacy/legacy-claim-card.md): the printable safety net.

### Premium features

- [Premium Features](./premium-features/README.md): what Free and Premium each include, the price, and how to buy.
- [Backup Beneficiaries and Backup Heirs](./premium-features/manage-contingent-beneficiaries.md): who can claim when the first people do not.
- [Name an Executor](./premium-features/executor.md): someone who can tell your legacy that you have died.
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

- [Shield Vault (protect a holding)](./shield.md): a wait on every withdrawal, and a recovery sheet that can stop one you did not start, if it is used before the wait ends.
- [Wallet alerts and the Bitcoin watch](./wallet-alerts.md): an email when a wallet moves, free for your own; Bitcoin addresses in your watch list.

## A note on philosophy

10102 is deliberately built so that your setup survives us. Every legacy, timelock and Shield Vault holding in this guide can be operated directly from the Ethereum contracts, without our UI. That's why [Legacy Claim Card](./legacy/legacy-claim-card.md) exists, and why [Architecture](../architecture/README.md) is published alongside the user guide. If something about our UI is confusing or our servers are down, your legacy doesn't stop working. That's the point.

Two things to know before you rely on anything here. The terms in force are v2026-12, published at [10102.io/disclosures](https://10102.io/disclosures); everyone is asked to accept them once more, and their text and hash can be checked against the chain at [app.10102.io/terms/current.json](https://app.10102.io/terms/current.json). And the newest contracts (the wallet legacy created since 2026-11-13, Shield Vault holdings opened since 2026-11-11) are live without an independent audit; their pages say what review they had.
