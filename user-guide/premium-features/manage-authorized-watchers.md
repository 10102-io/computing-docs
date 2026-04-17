---
description: >-
  Read-only accounts you authorize to view a legacy without being able to edit,
  delete, or alter it. Premium only.
---

# Manage Authorized Watchers

An **authorized watcher** is a read-only spectator on a legacy. They see it under **My Watchlist** in their 10102 dashboard; they can never edit, delete, activate, or otherwise affect it.

Watchers are a first-class way to coordinate with people who need visibility but shouldn't have control — estate lawyers, family members, advisors, compliance officers.

## Configuring

Go to **Settings → Authorized Watchers** and select a legacy to manage watchers for.

For each watcher you add:

- Their Ethereum address (EOA).
- A display name (private label for your own reference).
- A visibility level — see below.

You can add, remove, or change a watcher's visibility at any time.

{% hint style="warning" %}
**For Safe-backed legacies, only the creator can edit the watcher list.** The creator is the specific Safe signer who submitted the original creation transaction. Other Safe signers see the watcher list but with editing disabled and a "Notification settings managed by 0x…abcd" badge. This is documented in [Concepts — Creator vs. signer](../concepts.md#creator-vs-signer-safe-legacies); the workaround for handing off creator control is to delete and recreate with the new signer as submitter.
{% endhint %}

## Visibility levels

### Limited visibility (default, privacy-preserving)

With limited visibility:

- **Owner wallet address**: hidden. Replaced by a unique private identifier tied to the owner's 10102 account.
- **Safe co-signer addresses** (Multisig legacies): hidden.
- **Beneficiary addresses**: hidden. Names (owner-entered labels) are shown.
- **Watchers cannot locate the legacy on-chain** from what the app shows them — the identifier is 10102-internal, not an Ethereum address.

Use when: you want oversight without exposing the on-chain footprint. Typical for a general-purpose family watcher.

### Full visibility

With full visibility:

- All addresses are shown: owner, Safe co-signers (Multisig), beneficiaries.
- Watchers can look up the legacy contract on Etherscan and see the on-chain activity directly.
- Allocations, activation triggers, and asset lists are fully visible.

Use when: the watcher genuinely needs the addresses — typically an estate lawyer coordinating with off-chain documents, or a professional recovery service who needs to be able to act on-chain later.

## How watchers see the legacy

Watchers get a **My Watchlist** section in their dashboard. Each entry shows:

- The legacy's status (Needs finalizing, Live, Activated).
- Activity trigger and its remaining window.
- Asset allocations per beneficiary (at whatever visibility level they're granted).
- Any notes the owner has configured.

Watchers cannot act. No edit. No delete. No activation. They can only read.

## Contingent beneficiaries are automatically watchers

Second-line and third-line contingent beneficiaries are, by default, added as watchers with **limited visibility**. The owner can change their visibility at any time (e.g. upgrade an estate-lawyer contingent to full visibility).

- Contingents can't be removed from the watcher list directly — they're removed by editing the legacy itself (removing them from the contingent configuration).
- When their line activates, they move from **My Watchlist** to **My Inherited Legacy** with full-visibility beneficiary access.

## Real-world use cases

- **Consolidated oversight across wallets.** You manage several EOAs and Safes; watchers let a single dashboard see all of them without any one wallet having access to the others.
- **Estate coordination.** A lawyer references the plan from off-chain documents. Limited visibility preserves the owner's privacy while the unique identifier lets documents unambiguously point to a specific plan.
- **Family peace-of-mind.** A spouse or adult child has visibility without control — they know the plan exists, can see it's still active, but can't accidentally tamper.
- **Monitoring counterparty plans.** Professional setups where one party's 10102 legacy is a contingency for another party's obligations.

## See also

- [Configure Email Reminders](./configure-email-reminders.md)
- [Manage Contingent Beneficiaries](./manage-contingent-beneficiaries.md)
- [Concepts — Creator vs. signer (Safe legacies)](../concepts.md#creator-vs-signer-safe-legacies)
