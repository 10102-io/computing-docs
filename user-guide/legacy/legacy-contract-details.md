---
description: >-
  The details page is where you operate a legacy day-to-day: check status, add
  or remove assets, reset the activation timer, or tear it down.
---

# Legacy Contract Details

After creating a legacy, clicking it from the home page opens its details view. This is also where beneficiaries land when they open an inherited legacy.

## Sections of the details page

### Header

- **Name** and **Legacy ID** — the on-chain identifier, useful for claiming via Etherscan and for cross-referencing with emails.
- **Contract address** — where allocations, approvals, and native token deposits live.
- **Status** — see [Status values](#status-values) below.
- **Creator** (Safe legacies only) — the wallet that originally submitted the creation transaction. Relevant because notification settings (watchers, reminders) are creator-only; see the callout on the affected settings pages.

### Activity and trigger

- **Last activity** — your (or the Safe's) most recent outgoing transaction.
- **Activation trigger** — the inactivity threshold you configured. The details page shows how much of the window has elapsed.
- **Heartbeat action** — resets the trigger timer in a single transaction when you don't have another transaction to make naturally. Any outgoing transaction from your wallet (or Safe) already counts as a heartbeat; the dedicated action is for when you want a quick, explicit reset.

### Assets (Transfer legacies)

Shows the ERC-20s approved to the legacy contract, their current allowances, and the per-beneficiary distribution that would happen on activation.

- Only tokens with a non-zero approved amount are shown — keeping the view focused on what actually moves on activation. To see the full list of held tokens with zero allowance, enter edit mode.
- ETH is not shown in the assets table — ETH can't be approved as an ERC-20, so it's represented by whatever storage token (WETH, a liquid staking token, …) you swapped into. The home page shows your wallet's current ETH balance separately, independent of the legacy.

### Beneficiaries

The configured addresses, their allocations (for Transfer legacies), and — for Safe legacies — the role they'll take on activation.

### Danger zone

- **Edit contract** — adjust beneficiaries, allocations, trigger window, or asset approvals.
- **Delete contract** — tear the legacy down and return any native tokens; see [Edit or Delete](./edit-or-delete-a-legacy-contract.md).

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

- You'll see a banner if the legacy needs finalizing — your Safe co-signers can add signatures there, or at [app.safe.global](https://app.safe.global). Finalizing is a gas-paying transaction that any Safe signer can submit once the threshold is met.
- The heartbeat action is available to any Safe signer; the Safe's own transactions already reset the timer.
- Notification settings (watchers, reminders) are managed by the creator of the legacy, which may or may not be you. If it isn't, you'll see a "Notification settings managed by 0x…abcd" badge pointing to the creator. See [Creator vs. signer](../concepts.md#creator-vs-signer-safe-legacies).

### On an EOA legacy

- No finalizing step: on-chain changes happen as soon as you sign. Simpler flow, less coordination.
- Any outgoing transaction from your wallet already counts as a heartbeat. The dedicated action is a convenience when you haven't transacted in a while.

## The beneficiary's view (Inherited Legacy)

Beneficiaries see an inherited legacy on the home page under **My Inherited Legacy**, starting with status **Not activated**. The details page shows:

- **When they can activate** — based on the owner's last outgoing transaction and the activation trigger.
- **What they'd receive** — their allocation and the current list of approved assets (Transfer) or the Safe they'll inherit (Multisig).
- **Activation button** — enabled once the window has elapsed. Any designated beneficiary can trigger it; distribution happens to _all_ beneficiaries according to the allocations, not just the one who activated.

See [Activate a Legacy Contract and Claim Funds](./activate-a-legacy-contract-and-claim-funds.md) for the beneficiary flow.

## Watcher and contingent-beneficiary views

- A **watcher** sees the legacy under **My Watchlist** with either full or limited visibility, depending on what the owner configured. See [Manage Authorized Watchers](../premium-features/manage-authorized-watchers.md).
- A **contingent beneficiary** sees the legacy under **My Watchlist** until their line activates, then it moves to **My Inherited Legacy**.

## See also

- [Edit or Delete a Legacy Contract](./edit-or-delete-a-legacy-contract.md)
- [Legacy Claim Card](./legacy-claim-card.md)
- [Activate a Legacy Contract and Claim Funds](./activate-a-legacy-contract-and-claim-funds.md)
