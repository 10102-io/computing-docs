---
description: >-
  Opinionated shortcuts on the Legacy and Timelock pages that pre-fill a
  legacy or timelock for a specific goal. All of them drop into the standard
  flows, so you can tweak anything before signing.
---

# Quick Actions

Quick Actions are cards labelled by _what you want to accomplish_, not by product feature. Each one routes you into the relevant creation flow with sensible defaults so you don't have to compose the settings yourself.

## Where they are

- **Legacy page** (`app.10102.io/legacy`): a **Quick actions** section below your legacies, with the legacy shortcuts.
- **Timelock page**: a **Quick actions** section below your timelocks, with the timelock shortcuts and, where Guardian is available, an **Ask Guardian** button.

They show whether or not a wallet is connected; without one, the app asks you to connect (and to accept the terms) before it opens the form. Home does not carry them: it shows [your plan and your locks](./home.md) instead. Cypherpunk Mode has no Quick Actions; its pages open the create flows directly.

Use them as training wheels, or as actual product: they're not a cut-down flow, just a pre-configured one. Everything is editable before you sign the transaction.

{% hint style="info" %}
Quick Actions only pre-fill settings. They don't change what gets deployed on-chain. The result is indistinguishable from a hand-configured legacy or timelock of the same type. Guardian's setup links pre-fill the same forms in the same way.
{% endhint %}

## On the Legacy page

### Check in

Shown when you have a live wallet legacy. Opens that legacy's page, where **Check in** resets its countdown.

### Add a backup wallet

Pre-fills a **wallet legacy** named "My Backup Wallet": your assets go to a fallback address you choose if your wallet goes quiet. Takes you to the configure screen to add that address and pick which assets to include.

Use when: you want a second wallet to receive everything if you lose access to this one, or a straightforward one-person legacy.

A wallet holds one wallet legacy at a time, so this card is disabled, with the reason shown, while the app checks for an existing one or once it finds one.

### Designate future signers for my Safe

Pre-fills a **Multisig legacy** named "My Safe Legacy". Because this flow needs to know _which_ Safe you're targeting, you're first taken to the Safe verification step; once a Safe address is confirmed, the rest of the configure screen is pre-filled.

Use when: your Safe is your long-term vault and you want other people (family, a trusted proxy) to become co-signers if it goes quiet.

## On the Timelock page

### Lock funds for a fixed period

Pre-fills a standard **Timelock** named "My Timelock": no one, including you, can touch the funds until the period ends. You choose the period.

### Start a 2-year savings fund for a loved one

Pre-fills a **Beneficiary Timelock** named "Savings Fund", locked for 24 months, then released to someone you choose, such as a child or a family member.

### Add a 2-week spending delay

Pre-fills a **Soft Timelock** named "My Spending Delay" with a 14-day waiting period: the funds stay locked until _you_ start an unlock, and then you wait 14 days.

Use when: you want protection against coercion or a stolen key, so nobody can drain the locked balance at once.

### Sending a gift

For a gift that opens on a date, the Timelock page has a **Send a sealed gift** button instead of a card. It opens [Heirloom](./heirloom.md): occasion designs, printable sheets, and free claiming for the recipient.

## Gotchas

- **Quick Action ≠ skipping asset permissions.** Including assets still works exactly like the standard create: you pick tokens in the configure screen and the permission rides the single create transaction. See [Create a Legacy Contract](./legacy/create-a-legacy-contract.md) for what your wallet will show.
- **The Safe quick action requires a Safe.** You'll get routed to the Safe check first. If you don't have a Safe, create one at [app.safe.global](https://app.safe.global) first.
- **Nothing is irreversible before signing.** You can change beneficiaries, allocations, durations and names. Quick Actions are defaults, not locks.

## See also

- [Create a Legacy Contract](./legacy/create-a-legacy-contract.md): the full creation flow that Quick Actions drop into.
- [Using Timelock](./timelock/using-timelock.md): details on the three timelock flavors.
- [Concepts](./concepts.md): vocabulary reference.
