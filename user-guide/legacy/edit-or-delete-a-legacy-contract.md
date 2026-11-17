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

- **Name**: private label. On a wallet legacy it is optional, so a legacy without a name can be edited and saved as it is.
- **Beneficiaries**: add, remove, or replace up to 32 addresses.
- **Allocations**: any percentages, as long as they sum to 100%. Single-beneficiary legacies auto-allocate 100%.
- **Activation trigger**: the inactivity window, in months.
- **Note to beneficiaries**: a message they'll see in the beneficiary view.
- **Tokens for your legacy** (EOA legacies): **Approve** next to a token adds it. On a legacy created with the one-transaction flow it gives the same standing permission a ticked token gets at creation (Permit2, to the legacy's vault): there is no amount to pick or to raise later, and your wallet asks twice if it has never used Permit2 for that token. A token your wallet holds in full in the legacy reads **All in your legacy**. On an older legacy, **Approve** sets an amount, so approve again when your balance grows. A legacy that has ended gets no standing permission.
- **ETH** (EOA legacies): **Add ETH to your legacy** converts ETH into a storage token the legacy can use; on legacies created since 2026-11-07, **Convert all back to ETH** converts the whole stored-token balance in your wallet back. See [Create a Legacy Contract: Adding ETH](./create-a-legacy-contract.md#adding-eth).
- **Safe threshold for beneficiaries** (Multisig legacies only): how many beneficiaries must sign as the new Safe's threshold after activation.
- **Backup beneficiary and second backup** (Premium): add, remove or replace them, or change their waits. See [Backup Beneficiaries and Backup Heirs](../premium-features/manage-contingent-beneficiaries.md).

NFTs, backup heirs and the executor of a wallet legacy of the new version are not on the edit form: they are set on the legacy's own page ([Legacy Contract Details](./legacy-contract-details.md)).

On an EOA legacy, changes to the name, the beneficiaries' names and the note are saved on your device, not on the blockchain, so changing only them sends no transaction. If you [sign in on each device](../authentication.md#sign-in-on-each-device), the names (not the note) follow your wallet to your other devices.

### EOA legacy

A single transaction from your wallet applies the change. No coordination needed. After saving, the app opens that legacy's page.

### What an edit resets on the new version

On a wallet legacy of the new version, backup heirs and NFTs are tied to your heirs: each backup stands in for one heir, and each NFT goes to one heir. So a save that sends your heirs to the contract again **turns backup heirs off and clears the NFT list**. You set them again on the legacy's page afterwards.

| This edit | Backup heirs and the NFT list |
|---|---|
| Changing who your heirs are, or their shares | Removed |
| Adding or removing a backup beneficiary or a second backup, or changing its wait | Removed (that save also saves your heirs again) |
| Replacing the person who is your backup beneficiary or second backup | Kept |
| Changing only the check-in period | Kept |
| Changing the name, a beneficiary's name or the note | Kept (nothing is sent) |

The app warns first. While you edit a legacy that has backup heirs or NFTs, a note on the form says which changes remove them. When the save you are about to send does, the note reads "Saving this turns off backup heirs and clears the NFT list", and a dialog asks **Save anyway** or **Go back** before anything is sent. If the app cannot check, it sends nothing and asks you to try again. After such a save, the legacy's page says what was removed.

In the app's words, "Everything else you set stays."

{% hint style="info" %}
**Which edits reset the activation timer?** Checking in resets the countdown, and so does any change you make to this legacy (beneficiaries, allocations, trigger window). Sending, swapping or signing elsewhere does not, so checking in is up to you. With Premium, [automatic renewal](../premium-features/automatic-renewal.md) can count that activity too. Token approvals are made in your wallet, on the token, so they are not a change to the legacy and do not reset the timer; neither do the name and the note, which are saved on your device. To reset without another edit, use **Check in**.
{% endhint %}

### Safe legacy

Any Safe signer can submit the edit; your Safe co-signers then finalize at the Safe's threshold. The edit shows as _Needs finalizing to update_ until the threshold is met. You can finalize in our app or on [app.safe.global](https://app.safe.global).

## Deleting

Deletion is available to owners (EOA) or any Safe signer (Safe), and returns the legacy to a clean state:

- **Native tokens** held by the contract (rare: Transfer legacies rarely hold native tokens directly; this mostly applies to legacy contracts that pre-date the storage-token flow) are returned to the owner's wallet.
- **Token permissions** the legacy had are cleared after the delete, in your wallet:
  - A Permit2 permission to the legacy's vault, or in the legacy's own name (the one-transaction flow): one request clears all of them at once. The deleted legacy can no longer use such a permission, but it would stay open in Permit2, and a new legacy from the same wallet would include those tokens without asking you. That is why it is cleared.
  - A direct per-token approval to the legacy (older legacies): one `approve(legacy, 0)` request per token.

### EOA delete

1. Click **Delete contract** on the details page.
2. Confirm in the popup with **Delete now**.
3. Before the first wallet request, the app tells you how many are coming, for example: "Your wallet asks you to confirm 3 times: once to delete this legacy, then 2 times to clear the token permissions it had." A legacy with nothing to clear says "Your wallet asks you to confirm once, to delete this legacy. It has no token permissions to clear."
4. Confirm the delete, then each clearing. Each one is confirmed on chain before the next is asked.

The popup has a **Skip allowance revocation** box, for owners who prefer to revoke in their wallet themselves: with it ticked, only the delete is asked.

What the app says at the end:

- "Legacy deleted, and the token permissions it had are cleared." when every clearing went through.
- "Legacy deleted." with a warning when a clearing was declined or failed: "Some token permissions this legacy had could not be cleared. In your wallet or a revoke tool, revoke what is listed for this legacy's address or under Permit2. You can also clear them from Home."
- If the network reverted the delete itself, the app says the legacy was not deleted, and nothing is cleared.

A permission that could not be read (a token that does not answer, a network hiccup) is passed over in silence: the warning shows only for a clearing that was really asked of your wallet and did not go through.

### Permissions left open after a delete

If a clearing was declined, the tab was closed, or you ticked the skip box, the permissions stay open. The app keeps a pointer to them in this browser, and Home and the Legacy page show a note: "Token permissions from a legacy you deleted are still open.", why it matters, and how many wallet requests clearing takes. **Clear permissions** clears them in one go; **Dismiss** hides the note.

The chain decides what is shown. Once the chain shows nothing open, whoever cleared it and wherever, the note goes by itself. A Permit2 permission to the vault is not offered for clearing once the vault serves another legacy of your wallet that is live or was activated. The pointer lives in the browser where you deleted the legacy: on another device, use your wallet or a revoke tool, and look under the deleted legacy's address and under Permit2.

{% hint style="success" %}
**Post-delete refresh.** The Legacy page and the "create legacy" eligibility check re-read directly from the chain after a delete, so you can create a new legacy immediately without refreshing the page.
{% endhint %}

### Safe delete

1. Click **Delete contract** on the details page.
2. Confirm in the popup; this submits the first signature.
3. Safe co-signers finalize at the threshold; status shows _Needs finalizing to delete_ until done.
4. Once finalized, assets return to the Safe and approvals are revoked.

## Moving instead of deleting

If you want what the new version of the wallet legacy adds (NFTs, backup heirs, an executor) on a legacy created before 2026-11-13 20:42 UTC, do not delete and recreate by hand: the guided move removes the old legacy and creates the new one with the same heirs and shares, and checks first that the new one would be accepted. See [Move a Legacy to the New Version](./move-to-the-new-version.md).

## What stays the same after edits

Notification settings (watchers and email reminders) are set per legacy and controlled by the wallet that created it. Editing or replacing beneficiaries doesn't change who controls notifications. For Safe legacies where the creator is a specific Safe signer, only that signer can change notification settings; see [Creator vs. signer](../concepts.md#creator-vs-signer-safe-legacies).

If the creator needs to effectively hand off notification settings to another Safe signer, the current workaround is: delete the legacy and recreate it, with the new signer submitting the creation transaction.

## See also

- [Legacy Contract Details](./legacy-contract-details.md)
- [Move a Legacy to the New Version](./move-to-the-new-version.md)
- [Activate a Legacy Contract and Claim Funds](./activate-a-legacy-contract-and-claim-funds.md)
- [Manage Authorized Watchers](../premium-features/manage-authorized-watchers.md)
