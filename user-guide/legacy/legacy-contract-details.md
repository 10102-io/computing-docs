---
description: >-
  The details page is where you operate a legacy day-to-day: check status, add
  or remove assets, reset the activation timer, or tear it down.
---

# Legacy Contract Details

After creating a legacy, clicking it from the Legacy page (or its timer in **Your setup** on [Home](../home.md)) opens its details view. This is also where beneficiaries land when they open a legacy left to them.

## Sections of the details page

### Header

- **Name** and **Legacy ID**: the on-chain identifier, useful for claiming via Etherscan and for cross-referencing with emails. A wallet legacy may have no name (it is optional, and it stays on the device where you typed it); the page is then titled "Legacy Details".
- **Contract address**: where allocations, approvals, and native token deposits live.
- **Status**: see [Status values](#status-values) below.
- **Creator** (Safe legacies only): the wallet that originally submitted the creation transaction. Relevant because notification settings (watchers, reminders) are creator-only; see the callout on the affected settings pages.

### Activity and trigger

- **Last activity**: the most recent activity the legacy knows about. For Safe legacies that's the Safe's last transaction (every Safe transaction counts). For EOA legacies it's your last interaction with the legacy itself; see [The owner's view](#on-an-eoa-legacy) below.
- **Activation trigger**: the inactivity threshold you configured. The details page shows how much of the window has elapsed.
- **Check in** (EOA legacies): resets the trigger timer in a single transaction when you don't have another qualifying action to make naturally. On a Safe legacy, any Safe transaction already counts. On an EOA legacy, a check-in and any change you make to the legacy count, and nothing else does unless you enable [automatic renewal](../premium-features/automatic-renewal.md), so **Check in** is the standard way to stay active. With Premium, it is a free signature ([Gas-Free Check-ins & Claims](../premium-features/gas-free-check-ins-and-claims.md)). Once the window has run out and beneficiaries can already claim, the button reads **Reset countdown**: it still works until the first claim.

### Assets (Transfer legacies)

Shows the ERC-20s approved to the legacy contract, their current allowances, and the per-beneficiary distribution that would happen on activation.

- Only tokens with a non-zero approved amount are shown, keeping the view focused on what actually moves on activation. To see the full list of held tokens with zero allowance, enter edit mode.
- ETH is not shown in the assets table: ETH can't be approved as an ERC-20, so it's represented by whatever storage token (WETH, a liquid staking token, …) you swapped into. On Home, **Your setup** says how much of the wallet is not in your legacy yet, and why; most often it is ETH.
- With [Hide amounts](../hide-amounts-and-cypherpunk-mode.md#hide-amounts) on, the amounts on this page show as four dots.

### Beneficiaries

The configured addresses, their allocations (for Transfer legacies), and, for Safe legacies, the role they'll take on activation.

### NFTs, Backup heirs, Executor (wallet legacies of the new version)

The owner of a wallet legacy created since 2026-11-13 20:42 UTC sees three more sections. Each leads with what is set and has one main action: NFTs show "None" or "2 named", backup heirs "Off" or "On", the executor "None", "Named" or, while a confirmation is pending, "Death confirmed".

- **NFTs**: "Hand on NFTs too. Choose the NFT and the person it goes to. It stays in your wallet until they claim it." **Add NFTs** opens the editor; with NFTs named, the section lists each one with its heir and whether it is ready, and offers **Change NFTs**. See [NFTs in a Legacy](./nfts-in-a-legacy.md).
- **Backup heirs** (Premium): **Add backups** gives each heir up to two backups for their own share. Once they are on, the section shows a link to the **Heir page** to send to your backups. See [Backup Beneficiaries and Backup Heirs](../premium-features/manage-contingent-beneficiaries.md#backup-heirs-for-each-share).
- **Executor** (Premium): **Name executor** names someone who can tell this legacy you have died, and gives you the link to their **Executor page**. If they confirm, this section says so, with the new claim date and a **Check in** button that cancels it. See [Name an Executor](../premium-features/executor.md).

Without Premium, the last two say "Backup heirs are part of Premium." and "An executor is part of Premium.", with **See Premium**. If your last edit saved your heirs again, a note at the top says that backup heirs were turned off or the NFT list was cleared, and asks you to set them again; see [what an edit resets](./edit-or-delete-a-legacy-contract.md#what-an-edit-resets-on-the-new-version).

### A new version is available (older wallet legacies)

The owner of a live wallet legacy created before 2026-11-13 20:42 UTC sees a card instead: **A new version is available**, with **See what moving changes**. Nothing changes unless you move it. See [Move a Legacy to the New Version](./move-to-the-new-version.md).

### Danger zone

- **Edit contract**: adjust beneficiaries, allocations, trigger window, or asset approvals.
- **Delete contract**: tear the legacy down and return any native tokens; see [Edit or Delete](./edit-or-delete-a-legacy-contract.md).

## Status values

| Status | Meaning | Typical next step |
|---|---|---|
| **Needs finalizing** (Safe only) | A Safe transaction has been submitted but not enough signers have finalized it | Collect the remaining signatures in our app or on [app.safe.global](https://app.safe.global) |
| **Live** | The legacy is active; allocations are in effect; the activation trigger is counting down | Include assets, manage watchers, etc. |
| **Activated** | A beneficiary has successfully activated the legacy and distribution has started (Transfer) or completed (Multisig) | For Transfer with >100 transactions, keep claiming until done |

{% hint style="info" %}
**"Needs finalizing" threshold is inclusive.** When the number of signers who've approved equals or exceeds the Safe's threshold, the transaction is ready to finalize. (This was recently aligned with Safe's own semantics, so having _more_ signatures than strictly needed no longer blocks finalization.)
{% endhint %}

## The owner's view

### On a Safe legacy

- You'll see a banner if the legacy needs finalizing. Your Safe co-signers can add signatures there, or at [app.safe.global](https://app.safe.global). Finalizing is a gas-paying transaction that any Safe signer can submit once the threshold is met.
- The heartbeat action is available to any Safe signer; the Safe's own transactions already reset the timer.
- Notification settings (watchers, reminders) are managed by the creator of the legacy, which may or may not be you. If it isn't, you'll see a "Notification settings managed by 0x…abcd" badge pointing to the creator. See [Creator vs. signer](../concepts.md#creator-vs-signer-safe-legacies).

### On an EOA legacy

- No finalizing step: on-chain changes happen as soon as you sign. Simpler flow, less coordination.
- **Without automatic renewal**, the page says: "Checking in here resets the countdown, and so does any change you make to this legacy. Sending, swapping or signing elsewhere does not, so checking in is up to you. With Premium, automatic renewal can count that activity too."
- **With automatic renewal on** (Premium, opt-in, per legacy), it says: "Automatic renewal is on. If this wallet has sent a transaction since the countdown last restarted, we reset it for you in the last 30 days before your deadline. This works for up to 12 months after your last check-in and while Premium is active. Then check in yourself once and it carries on. You can check in yourself at any time." See [Automatic Renewal](../premium-features/automatic-renewal.md).
- **Check in** costs network gas. With Premium, it is a free signature and 10102 pays the fee; if that is not possible right now, your wallet sends it and you pay the gas.

## The beneficiary's view (Legacies for Me)

Beneficiaries see a legacy left to them on the Legacy page under **Legacies for Me**, starting with status **Not activated**. The details page shows:

- **When they can activate**: based on the owner's last recorded activity and the activation trigger.
- **What they'd receive**: their allocation and the current list of approved assets (Transfer) or the Safe they'll take over (Multisig).
- **Activation button**: enabled once the window has elapsed. Any designated beneficiary can trigger it; distribution happens to _all_ beneficiaries according to the allocations, not just the one who activated.

On a wallet legacy of the new version, an heir also sees their part on this page: their share, each share's turns when backup heirs are on, and the NFTs set aside for them, with a button to claim those NFTs once they can move. The same view is on the public **heir page**, which is also where a backup heir of a single share comes in.

See [Activate a Legacy Contract and Claim Funds](./activate-a-legacy-contract-and-claim-funds.md) for the beneficiary flow.

## Watcher and backup-beneficiary views

- A **watcher** sees the legacy under **My Watchlist** with either full or limited visibility, depending on what the owner configured. See [Manage Authorized Watchers](../premium-features/manage-authorized-watchers.md).
- A **backup beneficiary** or **second backup** (the legacy-wide layers) sees the legacy under **My Watchlist** until their period starts, then it moves to **Legacies for Me**.
- A **backup heir** of one share (new version) is not in either list. The owner sends them the heir page link, `app.10102.io/heir/1/<legacy address>`, where they connect the wallet that was named, see when their turn starts, and claim.

## See also

- [Edit or Delete a Legacy Contract](./edit-or-delete-a-legacy-contract.md)
- [Legacy Claim Card](./legacy-claim-card.md)
- [Activate a Legacy Contract and Claim Funds](./activate-a-legacy-contract-and-claim-funds.md)
