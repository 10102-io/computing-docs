---
description: >-
  Change allocations, add or remove beneficiaries, adjust the activation
  trigger, or tear a legacy down entirely. EOA changes are instant; Safe
  changes go through your normal Safe threshold.
---

# Edit or Delete a Legacy Contract

Once a legacy is **Live**, the owner (or, for Safe legacies, any Safe signer) can edit or delete it at any time before it's activated.

## Editing

### What you can change

- **Name** — private label.
- **Beneficiaries** — add, remove, or replace up to 10 primary addresses.
- **Allocations** — any percentages, as long as they sum to 100%. Single-beneficiary legacies auto-allocate 100%.
- **Activation trigger** — the inactivity window (days).
- **Note to beneficiaries** — an on-chain message they'll see in the inherited view.
- **Approvals** — include new ERC-20s, adjust existing allowances, or remove tokens from the inclusion set.
- **Safe threshold for beneficiaries** (Multisig legacies only) — how many beneficiaries must sign as the new Safe's threshold after activation.
- **Contingent layers** (Premium) — add, remove, or resize the second- and third-line activation windows.

### EOA legacy

A single transaction from your wallet applies the change. No coordination needed.

{% hint style="info" %}
**Which edits reset the activation timer?** Approving, adjusting, or removing allowances and changing the note **do not** reset the timer. All other edits (beneficiaries, allocations, trigger window) do. If you want to explicitly reset without another edit, use the heartbeat action instead.
{% endhint %}

### Safe legacy

Any Safe signer can submit the edit; your Safe co-signers then finalize at the Safe's threshold. The edit shows as _Needs finalizing to update_ until the threshold is met. You can finalize in our app or on [app.safe.global](https://app.safe.global).

## Deleting

Deletion is available to owners (EOA) or any Safe signer (Safe), and returns the legacy to a clean state:

- **Native tokens** held by the contract (rare — Transfer legacies rarely hold native tokens directly; this mostly applies to legacy contracts that pre-date the storage-token flow) are returned to the owner's wallet.
- **ERC-20 allowances** granted to the legacy contract are revoked automatically as part of the delete flow. This is a separate `approve(legacy, 0)` per-token; the UI runs through each one.

### EOA delete

1. Click **Delete contract** on the details page.
2. Confirm in the popup.
3. Sign one transaction to tear down the legacy. The UI then walks through revoking approvals on any tokens you'd approved.

{% hint style="info" %}
**If the revoke loop reports a warning.** You'll only see "some approvals could not be cleared" if an actual `approve(0)` transaction failed for a specific token. A legacy with zero tokens approved is deleted cleanly with no warning.
{% endhint %}

{% hint style="success" %}
**Post-delete refresh.** The home page and the "create legacy" eligibility check re-read directly from the chain after a delete, so you can create a new legacy immediately without refreshing the page.
{% endhint %}

### Safe delete

1. Click **Delete contract** on the details page.
2. Confirm in the popup — this submits the first signature.
3. Safe co-signers finalize at the threshold; status shows _Needs finalizing to delete_ until done.
4. Once finalized, assets return to the Safe and approvals are revoked.

## What stays the same after edits

Notification settings — **watchers and email reminders** — are tied to the wallet that created the legacy, not to the legacy itself. Editing or replacing beneficiaries doesn't change who controls notifications. For Safe legacies where the creator is a specific Safe signer, only that signer can change notification settings; see [Creator vs. signer](../concepts.md#creator-vs-signer-safe-legacies).

If the creator needs to effectively hand off notification settings to another Safe signer, the current workaround is: delete the legacy and recreate it, with the new signer submitting the creation transaction.

## See also

- [Legacy Contract Details](./legacy-contract-details.md)
- [Activate a Legacy Contract and Claim Funds](./activate-a-legacy-contract-and-claim-funds.md)
- [Manage Authorized Watchers](../premium-features/manage-authorized-watchers.md)
