---
description: >-
  Fallback beneficiary layers that activate only if the primary beneficiaries
  don't claim within a configurable window. Premium only.
---

# Manage Contingent Beneficiaries

Contingent beneficiaries are the answer to _"what if my primary beneficiaries can't claim?"_. Keys get lost. People become unreachable. People pass. The contingent layers are the safety net.

Premium users can configure up to **two** additional layers on top of their primary beneficiaries:

- **Second-line** — one designated address, activates after a configurable window following the primary activation.
- **Third-line** — one designated address, activates after a further configurable window following the second-line.

{% hint style="info" %}
**Only one contingent beneficiary per layer, and only EOA addresses.** Contingent beneficiaries are intentionally simple: a single address per fallback layer. If you need multi-party fallback logic, that's a case for a Multisig legacy at the primary level.
{% endhint %}

## When does each layer become eligible

Each layer has its own activation window — measured from the moment the _previous_ layer became eligible, not from the original trigger:

1. **Primary activation** — the legacy's main trigger elapses; primary beneficiaries can claim.
2. After primaries' window, **second-line** can claim (if configured).
3. After second-line's window, **third-line** can claim (if configured).

At any given time, multiple lines can be simultaneously eligible. The first address to actually claim "wins" the whole remainder — see below.

## Who actually gets the funds

- **Before second-line is eligible**: only primaries can claim, splitting according to the primary allocations.
- **Between second-line eligible and someone claiming**: primaries can _still_ claim with the primary allocations. If a primary moves first, second-line is irrelevant.
- **If the second-line claims**: they become the **sole beneficiary** and receive everything remaining. No primary can claim afterwards.
- **Same logic applies between second-line and third-line.**

The model is first-to-act-wins among currently eligible parties. This keeps the contract simple and avoids coordinated-distribution failure modes.

{% hint style="success" %}
**Why one person and not a split for contingents?** Because the fallbacks are typically a different _kind_ of person — an estate lawyer, a specialized recovery service, a single trusted proxy — and splitting their allocation doesn't match how those roles work in practice. If you want a split fallback, your primary layer is the right place to configure it.
{% endhint %}

## How contingent beneficiaries see the legacy

Until their line activates, contingent beneficiaries are treated as **watchers with limited visibility** — they see the legacy under **My Watchlist**, not **My Inherited Legacy**. The UI explicitly shows whether they're viewing as a watcher or as an imminent contingent beneficiary.

Once their line is eligible, the legacy moves to **My Inherited Legacy** and they get full-visibility beneficiary access.

{% hint style="info" %}
**You can change a contingent beneficiary's visibility.** The defaults are conservative (limited), but the owner can switch a contingent to full visibility from the watcher settings. Useful when the contingent is an estate lawyer who needs to see addresses to coordinate off-chain planning.
{% endhint %}

## Configuring

Contingent layers can be configured:

- **At creation** — during the configure step of a Transfer legacy.
- **Via edit** — at any time before the primary trigger has activated.

For each layer you set:

- The contingent's Ethereum address (EOA only).
- The window (in days) that must pass after the previous layer became eligible.

## Removing

- **Second-line and third-line beneficiaries** can't be removed from the watcher list directly; they're removed by editing the legacy itself.
- Removing a line of contingent activation also removes that beneficiary's watcher entry.

## Real-world example

> Alice sets up a Transfer legacy:
> - **Primaries**: her kids Bob and Charlie, 50% each, 180-day activation trigger.
> - **Second-line**: her estate lawyer Daria, 90-day window after primary.
> - **Third-line**: a professional recovery service, 180-day window after second-line.
>
> Alice goes silent. 180 days pass — Bob and Charlie can claim. 90 more days pass with no claim — Daria becomes eligible. If Bob finally checks his email and claims at that moment, the original 50/50 split to Bob and Charlie still happens. If Daria claims first, she gets everything (she's supposed to professionally recover and distribute off-chain).

## See also

- [Manage Authorized Watchers](./manage-authorized-watchers.md)
- [Activate a Legacy Contract and Claim Funds](../legacy/activate-a-legacy-contract-and-claim-funds.md)
