---
description: >-
  A wallet legacy created before 2026-11-13 20:42 UTC can move to the new
  version from its own page. The move keeps your heirs and their shares, and
  it is a new legacy: a new address, a timer that starts again, the fee of
  that day.
---

# Move a Legacy to the New Version

A legacy contract cannot be upgraded. So a wallet legacy created before 2026-11-13 20:42 UTC keeps its code and its screens for good, and keeps working exactly as it is. If you want what the new version adds, the app offers a guided move: the old legacy is removed and a new one is created with the same heirs and shares.

You do not have to move. Without Premium the move adds little: NFTs, from the three collections on the list, and on the oldest wallet legacies the ability to include USDT. Backup heirs and an executor are part of Premium. A move costs network gas and restarts your check-in timer, so read what changes before you start. Safe legacies are not part of this.

## Where it starts

The page of a live wallet legacy that is older than the new version shows a card: **A new version is available**. **See what moving changes** opens the move page for that legacy. The page lists what stays and what changes for your legacy, with its own names, dates and amounts, before anything is sent.

## What stays the same

Who gets what, and when:

- Your heirs and their shares.
- Your backup beneficiary and your second backup, with their waits, if you have them. Keeping them needs Premium on the day of the move.
- Your check-in period.
- The tokens in your legacy: the move gives the new legacy the permission for the same tokens again.

## What changes

- **A new contract address.** Your heirs see the new legacy in their list. Printed claim cards and saved links name the old address: print them again. See [Legacy Claim Card](./legacy-claim-card.md).
- **The claim fee is the one of the day of the move.** Each legacy keeps the fee of the day it was created, and a moved legacy is a new one. The page shows both rates when they differ. A legacy created before 2026-09-29 carries no fee, for example, and the new one would carry 0.25% today.
- **The check-in timer starts again.** The page shows the new claim date.
- **ETH the old legacy holds goes back to your wallet** when it is removed. After the move you can put it in the new legacy with one more confirmation.
- **ETH saved as a storage token.** If your old legacy converted that token back to ETH for your heirs, the new one does not: your heirs receive the token itself.
- **Reminder emails and automatic renewal** belong to the old legacy. Turn them on again on the new one. See [Configure Email Reminders](../premium-features/configure-email-reminders.md) and [Automatic Renewal](../premium-features/automatic-renewal.md).
- **Gas-free claims** are allowed on every new legacy. If you had switched them off on the old legacy, switch them off again on the new one.
- **Authorized watchers** are set per legacy contract, so the ones on the old legacy do not watch the new one. Add them again in Settings, Authorized watchers.
- **New:** you can add [NFTs](./nfts-in-a-legacy.md), and with Premium [backup heirs](../premium-features/manage-contingent-beneficiaries.md#backup-heirs-for-each-share) and an [executor](../premium-features/executor.md).

## How the move runs

Tick the terms box (the new legacy records your acceptance on-chain, like any create), then **Move my legacy**.

- **Wallets that batch do it in one transaction.** The old legacy is removed and the new one created in the same transaction, with your tokens in it again, so you are never without a legacy.
- **Other wallets confirm each step**, and the page says how many times: remove the old legacy, create the new one, then add your tokens again. A wallet holds one wallet legacy at a time, so the old one goes first. Between the first two confirmations you are without a legacy.

Before the first confirmation the app checks that the new legacy would be accepted: that new legacies can be created right now, that you have Premium if the legacy keeps a backup beneficiary, and that the removal and the create would go through together. If it cannot run that check, the move stops before anything is removed.

The move is refused, with the reason and nothing changed, when the new version would refuse your legacy as it is. For example: your heirs can already claim (check in first), an heir is also your backup beneficiary, a backup beneficiary has no wait, or your shares do not add up to 100%. Edit the legacy, then move it.

## If it stops halfway

If anything stops after the first confirmation (a declined request, a closed tab), Home, the Legacy page and the legacy's page show a notice until it is finished:

- **Finish the move**: the old legacy is removed and the new one is not created yet. You are without a legacy until it is. The page shows the claim fee again; if it changed since you started, it says so, and the button names the rate you would create at.
- **Finish adding your tokens**: the new legacy exists, and some tokens are not in it yet, so your heirs would not receive them.

If the create cannot happen, the app says that the old legacy is gone and opens the create form with the same heirs (**Create a legacy with the same heirs**).

The notice shows in the browser where the move began, for the same wallet and network.

## After the move

The page shows the new address. After the move, do what applies:

- Turn reminders and automatic renewal on again on the new legacy, and add your authorized watchers again.
- If you had switched gas-free claims off on the old legacy, switch them off again.
- Put back the ETH your old legacy held, if any, with the button the page shows.
- Print new claim cards and send your heirs the new links.
- Add NFTs, backup heirs or an executor on the new legacy's page, if you moved for them.

## Honest limits

- **Not independently audited.** The new version has had internal adversarial reviews and automated testing and analysis, and no independent audit. The two firm audits (reports of January 2025 and October 2025) were made on the contracts as they stood then. A wallet legacy created since then already runs code that was changed afterwards with internal review only (the one-transaction create, the vault for token permissions, automatic renewal). The new version adds more of it. Weigh that before you move.
- **A move cannot be undone.** The old legacy is removed. You can create another legacy later, but it will again be the new version.
- **Between two confirmations you may be without a legacy**, unless your wallet batches. Do not start a move you cannot finish in one sitting.

## See also

- [Legacy: the new version](./README.md#the-new-version-of-the-wallet-legacy)
- [Edit or Delete a Legacy Contract](./edit-or-delete-a-legacy-contract.md)
- [Create a Legacy Contract](./create-a-legacy-contract.md): the claim fee.
