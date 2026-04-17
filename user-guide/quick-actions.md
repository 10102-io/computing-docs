---
description: >-
  Opinionated shortcuts on the home page that pre-fill a legacy or timelock for
  a specific goal. All of them drop into the standard flows \u2014 you can tweak
  anything before signing.
---

# Quick Actions

Quick Actions are prominent buttons on the home page labelled by _what you want to accomplish_, not by product feature. Each one routes you into the relevant creation flow with sensible defaults so you don't have to compose the settings yourself.

Use them as training wheels, or as actual product: they're not a cut-down flow, just a pre-configured one. Everything is editable before you sign the transaction.

{% hint style="info" %}
Quick Actions only pre-fill settings. They don't change what gets deployed on-chain. The result is indistinguishable from a hand-configured legacy or timelock of the same type.
{% endhint %}

## Available actions

### Pass my assets to a loved one

Pre-fills a **Transfer legacy** with your connected wallet as the owner and a single beneficiary at 100% allocation. Takes you to the configure screen to add the beneficiary's address and pick which assets to include.

Use when: you want a straightforward one-person inheritance without thinking about allocations.

### Split my assets among several people

Pre-fills a **Transfer legacy** with allocation inputs for multiple beneficiaries. You enter the addresses and percentages.

Use when: you know this will be a multi-beneficiary setup and you'd rather start from a split-ready screen.

### Schedule a future gift (to a loved one)

Pre-fills a **Timelocked gift** with recipient, locked duration, and assets to include. The locked window is configurable in days.

Use when: you want scheduled delivery to another wallet on a specific date — planned giving, a child's milestone, a trust-fund-style disbursement.

### Protect all my funds (for a waiting period)

Pre-fills a **Soft Timelock**: locks your chosen assets indefinitely; when _you_ want them back, you initiate an unlock and a configurable waiting period begins. The window is measured in days.

Use when: you want anti-coercion protection — compromised keys or a wrench attack can't immediately drain the locked balance.

### Designate future signers for my Safe

Pre-fills a **Multisig legacy** configuration. Because this flow needs to know _which_ Safe you're targeting, you're first taken to the Safe verification step; once a Safe address is confirmed, the rest of the configure screen is pre-filled to highlight the signer-designation use case.

Use when: your Safe is your long-term vault and you want other people (family, a trusted proxy) to become co-signers if you go quiet.

## Gotchas

- **Quick Action ≠ shortcut through approvals.** Approval (asset inclusion) still happens in the standard second step, with the standard wallet prompts.
- **The Multisig quick action requires a Safe.** Don't click it from an EOA-only context; you'll get routed to the Safe check. If you don't have a Safe, create one at [app.safe.global](https://app.safe.global) first.
- **Nothing is irreversible before signing.** You can swap types (e.g. change Transfer to Multisig), change beneficiaries, change allocations. Quick Actions are defaults, not locks.

## See also

- [Create a Legacy Contract](./legacy/create-a-legacy-contract.md) — the full creation flow that Quick Actions drop into.
- [Using Timelock](./timelock/using-timelock.md) — details on the three timelock flavors.
- [Concepts](./concepts.md) — vocabulary reference.
