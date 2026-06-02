---
description: >-
  Activate a legacy left to you and claim funds. Works through the 10102 app,
  the Safe platform (for Safe legacies), or directly from any Ethereum
  interface using the printed Legacy Claim Card.
---

# Activate a Legacy Contract and Claim Funds

When a legacy's activation window has elapsed, any of its designated beneficiaries can activate it. This page covers all three paths: via the 10102 app, via the Safe platform (for Safe legacies), and via a blockchain explorer (for any legacy, using the Legacy Claim Card).

## Preflight

Before you can activate:

1. **Enough time must have passed** — the owner must have been inactive (no outgoing transactions from the owning wallet/Safe) for at least the configured trigger window.
2. **Your wallet must be one of the configured beneficiaries** — primary, or contingent after the corresponding line has kicked in.
3. **You need gas** — activation is a paid transaction. You pay, but the assets are distributed to _all_ beneficiaries according to the allocations, not just to you.

## Via the 10102 app

1. Open the app and go to **Legacies for Me**. Legacies left to you show up here whenever an address in them matches your connected wallet.
2. Click the legacy to open its details.
3. Click **Check contract's status & claim funds**.
4. If the window has elapsed, you're prompted to sign the activation transaction. Sign and pay gas.
5. The legacy's status moves to **Activated**.

### For Transfer legacies

All approved ERC-20s are distributed from the owner's wallet to the listed beneficiaries according to the allocations, and any native tokens the contract holds are distributed the same way.

{% hint style="info" %}
**Big legacies distribute in batches.** The contract can process up to 100 transfers per activation transaction. If the legacy you're claiming has more than that (e.g. 4 beneficiaries × 30 tokens = 120 transfers), a **Claim Remaining Fund** button stays available on the details page until everything is distributed. Any beneficiary can press it, any number of times.
{% endhint %}

### For Multisig legacies

Activation adds the beneficiaries as signers to the owner's Safe, with the threshold the owner configured. From that point on, the Safe is under your collective control — you can act on it in the normal Safe UI, our UI, or any other tool.

## Via the Safe platform (Safe legacies only)

Safe-backed legacies can be activated from [app.safe.global](https://app.safe.global) independently of our UI:

1. Open the Safe in app.safe.global.
2. Navigate to the Apps tab and open the 10102 Legacy app (or interact with the module directly, for advanced users).
3. Activate from there. The module does the same work as our UI.

Useful as a backup path if our UI is down, or if you already live in Safe's interface.

## Via a blockchain explorer (any legacy)

This is the path your Legacy Claim Card describes. You can reach it any time, with or without our UI.

1. Open an explorer like Etherscan for the network the legacy is on.
2. Navigate to the **Router contract** from the card — it's a **Transfer Legacy Router** (or EOA Router) for Transfer legacies.
3. Connect your wallet (beneficiary address).
4. On the _Contract → Write_ tab, call the activation function with the **Legacy ID** from the card as the argument.
5. Sign and pay gas.

The contract will distribute assets exactly as it would have done from the UI — the router has no knowledge of where the call originated.

{% hint style="success" %}
**You don't need a list of assets.** The router reads the approved token list from the legacy contract itself. You only need the Legacy ID.
{% endhint %}

## After activation

- **Status** reads **Activated** for everyone (owner, beneficiaries, watchers).
- **Watchers** still see the legacy under their watchlist for historical context.
- **Owner actions** are disabled — no more edits, no more deletes. The legacy has done its job.
- **Email notifications** (if configured) go out to impacted parties summarizing the distribution.

## Troubleshooting

- **"Not enough time has passed"** — your clock isn't the authoritative one. The contract checks against the owner's last outgoing transaction timestamp on-chain. Wait, or verify the owner really has been inactive for the trigger window.
- **"You are not a beneficiary"** — the address you're connected with doesn't match a configured beneficiary for this legacy ID. Re-check the card; check the address you expect to be listed; ask the owner if you suspect a typo.
- **"Insufficient gas"** — activation for a legacy with many tokens and beneficiaries can be expensive. If you're running low, coordinate with another beneficiary, or use the Claim Remaining Fund batching to split the cost.

## See also

- [Legacy Claim Card](./legacy-claim-card.md) — the printed safety net.
- [Concepts — Activation trigger](../concepts.md#activation-trigger).
