---
description: >-
  Who can claim when the people you named first do not: a backup beneficiary
  and a second backup for the whole legacy, and, on wallet legacies of the
  new version, up to two backups for each heir's own share. Premium only.
---

# Backup Beneficiaries and Backup Heirs

Backups are the answer to _"what if the people I named can't claim?"_. Keys get lost. People become unreachable. People pass.

There are two kinds, and they answer two different questions:

| | For the whole legacy | For one heir's share |
|---|---|---|
| The app calls them | **Backup beneficiary** and **Second backup** | **Backup heirs** (first backup, second backup of an heir) |
| The question | What if nobody claims at all? | What if this one heir cannot claim? |
| What they receive | Whatever a claim of theirs moves once their period has started | That heir's share |
| Available on | Transfer legacies, old and new | Wallet legacies of the new version (created since 2026-11-13 20:42 UTC, or moved to it) |
| Set where | The create form and the edit form | The legacy's own page, **Backup heirs** |
| Wait | In months: after the claim date for the backup beneficiary, after the backup beneficiary for the second backup | 30 to 3,650 days, the same for every heir |

Both need Premium to set or change. A wallet legacy of the new version can use both at once.

{% hint style="info" %}
**Older names.** Earlier versions of this guide, and the contracts, call the backup beneficiary and the second backup the second line and the third line, or contingent beneficiaries. They are the same thing.
{% endhint %}

## A backup beneficiary and a second backup

Up to **two** people behind your beneficiaries, one after the other:

- **Backup beneficiary**: one address. After a wait you set, counted from the claim date, this person can claim all that your wallet then holds of the tokens in your legacy.
- **Second backup**: one address, behind the backup beneficiary, after a further wait. The form offers it once a backup beneficiary is filled in.

In the form's words: "A safety net in case your beneficiaries never claim: after an extra waiting period, this backup person can claim the full remaining funds. Your main beneficiaries stay eligible the whole time."

{% hint style="info" %}
**One person per layer, and no Safe addresses.** A layer is a single wallet address, not a split, and it cannot be one of your beneficiaries, yourself or a Safe. If the person has no wallet yet, choose **Print a key for them**: the app makes a key on your device and a key sheet to download and hand over, like it does for a beneficiary without a wallet. The sheet is the only copy of that key.
{% endhint %}

### When each one becomes eligible

Each wait starts when the layer before it became eligible, not from the original trigger:

1. **The claim date**: the legacy's check-in period runs out; your beneficiaries can claim.
2. After the backup beneficiary's wait, the **backup beneficiary** can claim (if you named one).
3. After the second backup's wait, the **second backup** can claim (if you named one).

Several layers can be eligible at the same time. A claim settles only what it moves; see below.

### Who actually gets the funds

- **Before the backup beneficiary is eligible**: only your beneficiaries can claim, splitting according to their shares.
- **Once the backup beneficiary is eligible**: your beneficiaries can still claim with their shares, and the backup beneficiary can claim too. Each claim moves what your wallet holds of the named tokens at that moment: a beneficiary's claim splits it by shares, the backup beneficiary's claim sends all of it to the backup beneficiary.
- **A claim does not close the legacy to the others.** Tokens that were not in a claim, or that reach your wallet later, can be claimed by whoever claims next. If your wallet keeps receiving tokens after your beneficiaries claimed, the backup beneficiary can claim all of them once their period has started.
- **The same holds between the backup beneficiary and the second backup.**

Name as a backup only someone you would trust with everything, and choose the wait with that in mind. With backup heirs on, the rule is different: see [Using both](#using-both).

{% hint style="success" %}
**Why one person and not a split?** Because these fallbacks are typically a different _kind_ of person (an estate lawyer, a specialized recovery service, a single trusted proxy), and splitting does not match how those roles work in practice. If you want each of your heirs to have a stand-in of their own, that is what backup heirs are for.
{% endhint %}

### How they see the legacy

Until their period starts, a backup beneficiary and a second backup are treated as **watchers with limited visibility**: they see the legacy under **My Watchlist**, not **Legacies for Me**. Once their period starts, the legacy moves to **Legacies for Me** and they get full-visibility beneficiary access.

{% hint style="info" %}
**You can change their visibility.** The default is limited, but the owner can switch a backup to full visibility from the watcher settings. Useful when the backup is an estate lawyer who needs to see addresses to coordinate off-chain planning.
{% endhint %}

### Configuring

- **At creation**: in the create form of a Transfer legacy, under **Backup beneficiary (optional)** and **Second backup (optional)**.
- **Via edit**: at any time before the legacy is activated.

For each one you set a name (optional, kept on your device), the wallet address or a printed key, and the wait: "Activate beneficiary if no action occurs within", in months.

To remove one, edit the legacy and empty it. Removing it also removes that person's watcher entry.

{% hint style="warning" %}
**On a wallet legacy of the new version, adding or removing a backup beneficiary or a second backup, or changing its wait, turns backup heirs off and clears the NFT list.** That save also sends your heirs to the contract again, and both are tied to your heirs. Replacing the person keeps them. The edit form warns before it sends such a save; see [what an edit resets](../legacy/edit-or-delete-a-legacy-contract.md#what-an-edit-resets-on-the-new-version).
{% endhint %}

## Backup heirs for each share

On a wallet legacy of the new version, each heir can have up to two backups of their own. In the app's words: "Give each heir up to two backups. If an heir does not claim their share, a backup can, after a wait you choose. Each share is claimed on its own, so nobody waits for anyone else."

### How it works

- **Each heir's share is its own account.** The heir can claim it from the claim date, their first backup one wait later, their second backup two waits later.
- **One wait for the whole legacy**, 30 to 3,650 days. With a wait of 90 days, a first backup's turn starts 90 days after the claim date and a second backup's 180 days after it.
- **Whoever receives something from a share first, in their turn, keeps the rest of it**, later deposits included. If the heir claims, the share is theirs for good; if a backup claims first, in the backup's turn, it is the backup's.
- **Nobody waits for anyone else.** One heir who cannot claim does not hold up the others, and a backup takes only the share of the heir they stand in for.
- **With backups on, every heir receives tokens as they are**, never converted to ETH.
- **NFTs follow the same line.** An NFT set aside for an heir can be claimed by that heir's backup in the backup's turn.

### Rules the app checks

- A backup cannot be you, your executor, someone already named in the legacy, or the backup of two heirs. The two backups of one heir must be different people.
- Each of your heirs must appear once, with shares adding up to 100%, and none of them can also be your backup beneficiary or second backup. The section says why when it cannot turn backups on.
- If you also have a backup beneficiary, every backup heir's turn must start before the backup beneficiary takes over, so the backup beneficiary's wait must be at least two of the backup heirs' waits. The editor tells you the longest wait that fits.
- A share with no backups stays with its heir: leave its fields empty.

### Setting them

On the legacy's page, in the **Backup heirs** section:

1. Press **Add backups** (or **Change backups**).
2. Type the **Wait between an heir and each backup (days)**.
3. For each heir, type the wallet address of the **First backup** and, if you want one, the **Second backup (optional)**.
4. Press **Save backups** and confirm in your wallet. Saving counts as a check-in.

**Turn off** removes all of them in one transaction, and needs no Premium. Changing them does.

Once backups are on, the section shows the link to the legacy's **Heir page**: "Send your backups this page, where they see their turn and claim." Backup heirs are not in the indexed list of heirs that **Legacies for Me** is built from, so send them that link: it is how they find the legacy. See [the heir page](../legacy/activate-a-legacy-contract-and-claim-funds.md#the-heir-page-new-version).

### What turns backup heirs off

Any change to your heirs or their shares turns backup heirs off, and so does adding or removing a backup beneficiary or a second backup, or changing its wait. You set them again afterwards. The edit form warns first, and the legacy's page says so after such a save; see [what an edit resets](../legacy/edit-or-delete-a-legacy-contract.md#what-an-edit-resets-on-the-new-version).

## Using both

On a wallet legacy of the new version the two kinds work together:

- Backup heirs come first, share by share.
- The backup beneficiary, and then the second backup, take only what nobody has received from: a share that already paid its heir or a backup stays with that person.

## Real-world example

> Alice sets up a wallet legacy:
> - **Heirs**: her kids Bob and Charlie, 50% each, with a 6-month check-in period.
> - **Backup heirs**, wait 90 days: Bob's wife Dana for Bob's share, Charlie's partner Eli for Charlie's share.
> - **Backup beneficiary**: her estate lawyer Daria, 1 year after the claim date.
>
> Alice goes silent. After 6 months, Bob and Charlie can each claim their half. Bob claims his. Charlie has lost his keys. 90 days after the claim date, Eli can claim Charlie's half, and does. Daria's year passes, and nothing is left for her to claim: both shares went to someone. Had nobody claimed Charlie's half, Daria could have claimed it after her year (she is supposed to recover it professionally and distribute it off-chain).

## Honest limits

- **Nobody checks why a share was not claimed.** A backup heir can claim a share once their wait has passed, unless someone ahead of them already received from it. Without backup heirs, a backup beneficiary or a second backup can claim once their period has started, whether or not your beneficiaries claimed before. A claim cannot be undone. Choose waits long enough for an heir who is slow, not gone.
- **Backup heirs exist only on the new version of the wallet legacy**, which is live without an independent audit: internal adversarial reviews and automated testing and analysis only. See [Legacy](../legacy/README.md#honest-limits-of-the-new-version).
- **Premium is needed to set or change them.** Turning backup heirs off is always possible.

## See also

- [Name an Executor](./executor.md)
- [Manage Authorized Watchers](./manage-authorized-watchers.md)
- [Activate a Legacy Contract and Claim Funds](../legacy/activate-a-legacy-contract-and-claim-funds.md)
