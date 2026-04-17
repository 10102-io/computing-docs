---
description: >-
  Set up a legacy contract so your assets can pass to your beneficiaries on your
  terms, without a middleman.
---

# Create a Legacy Contract

A legacy contract holds the rules for how, when, and to whom your assets pass. This page walks through creating one end-to-end.

{% hint style="info" %}
**New to the terminology?** See [Concepts](../concepts.md) for short definitions of _legacy type_, _storage token_, _activation trigger_, and _claim card_.
{% endhint %}

## Pick a type

You have two choices at `Create a legacy`:

- **Transfer legacy** — splits your assets between named Ethereum addresses when activated. Your wallet can be an EOA (MetaMask, Ledger, Trezor, …) _or_ a Safe.
- **Multisig legacy** — hands over control of your existing **Safe** to your beneficiaries by adding them as co-signers once activated.

Still unsure? Start with Transfer legacy — it's the most common case and doesn't require a Safe. You can always create a Multisig legacy later.

{% hint style="success" %}
If you started from a **Quick action** on the home page, we've pre-filled the type and some settings for you. The rest of this page still applies. See [Quick Actions](../quick-actions.md) for the full list.
{% endhint %}

## Create a Transfer legacy — connected wallet (EOA)

This is the fastest path: your own wallet stays in full control; the legacy contract only gets permission to move assets once activated.

### Step 1 — Deploy the contract

Fill in:

- **Name** — private label, visible only to you and the beneficiaries you invite.
- **Beneficiaries** — up to 10 addresses. If you list only one, we auto-allocate 100%.
- **Allocations** — percentages must sum to 100%.
- **Activation trigger** — inactivity period (time since your last outgoing transaction) before beneficiaries can claim. Days, not months.

Click **Deploy**. Your wallet will prompt a single transaction to create the contract via our `LegacyDeployer` using `CREATE2`, meaning the final contract address is deterministic — you'll see it before you sign.

{{screenshot: configure-legacy-eoa-step1}}

### Step 2 — Approve the assets

After deployment, the details page opens. Nothing has been moved yet; your wallet still holds everything. To decide what gets distributed on activation, you approve the legacy contract as a spender.

For each ERC-20 token: use the **include** action on the token row. Your wallet prompts a standard `approve(legacyAddress, amount)` — normal ERC-20 semantics, no unusual permissions.

For ETH: ETH can't be approved directly because it isn't an ERC-20. Use the **add ETH** action to swap it to a **supported storage token** (WETH or a liquid staking token) through a built-in swap route, then approve the resulting token. This is a two-wallet-prompt flow; both prompts are expected.

{% hint style="warning" %}
**Why does my wallet warn me about the second transaction?** Some wallets (MetaMask, Rabby, Rainbow, …) flag the swap's approval step as "possibly suspicious" because the legacy contract is new to the wallet — there's no prior transaction history with it. That's expected for a brand-new contract. You can verify the spender address matches the legacy address shown at the top of the details page. The warning goes away once the contract has some history with your wallet.
{% endhint %}

{% hint style="info" %}
**Partial approvals are fine.** You control the allowance amount. To include only 50% of your USDC, approve only 50%. Your beneficiaries can only ever claim what's approved; the rest stays spendable by you as usual.
{% endhint %}

### Step 3 — Print a Legacy Claim Card (optional but recommended)

The app generates a one-page printable card with the minimum information your beneficiaries need to claim — even if our UI is ever unavailable. See [Legacy Claim Card](./legacy-claim-card.md).

## Create a Transfer legacy — Safe wallet

Same logical flow as the EOA path, with one difference: every on-chain change (deploy, approvals, edits) is a Safe transaction that your other signers must approve according to your Safe's threshold.

1. Choose **Use a Safe account** when prompted.
2. Enter the Safe address. We verify it's a Safe contract on the current network.
3. Configure the contract as above. Submit.
4. Your Safe co-signers sign in their usual flow (10102 app or `app.safe.global`). Once the Safe threshold is met, the transaction executes and the legacy is live.
5. Approve assets the same way — each approval is a Safe transaction.

{% hint style="info" %}
**Notification settings (watchers, email reminders) are tied to the wallet that _submitted_ the creation transaction.** Other Safe signers can sign on-chain edits later, but only the original submitter can manage notifications. See [Manage Authorized Watchers](../premium-features/manage-authorized-watchers.md) for details.
{% endhint %}

## Create a Multisig legacy

Multisig legacy _is_ your Safe — beneficiaries become co-signers on activation, inheriting the wallet and any positions it holds elsewhere (staking, governance, NFTs, etc.).

1. Click **Create a legacy**, choose **Multisig legacy**.
2. Enter your Safe address. We verify it.
3. Configure:
   - Contract name, beneficiaries (addresses), activation trigger.
   - No asset allocation here — beneficiaries inherit the Safe itself.
4. Your Safe signers approve the creation transaction (installs our Safe module and guard).

{{screenshot: configure-multisig-check-safe}}

{% hint style="warning" %}
The Safe must still be in active use — Multisig legacy relies on your Safe's last outgoing transaction as a liveness signal. If you don't transact from the Safe at least once per activation window, beneficiaries can activate. Use the heartbeat action on the details page to reset the timer if you don't have another transaction to send.
{% endhint %}

## Premium: Additional contingent beneficiaries

If you subscribe to Premium, you can configure up to two additional contingent layers — fallbacks if a primary beneficiary can't claim (lost keys, deceased, etc.). See [Manage Contingent Beneficiaries](../premium-features/manage-contingent-beneficiaries.md).

## Common questions

**Can I add ETH directly to the legacy?** Not for Transfer legacies — ETH must be swapped to a storage token first. This is by design: the legacy contract uses ERC-20 allowances, which ETH doesn't have.

**What happens if I move my approved tokens?** Your allowance stays, but beneficiaries can only claim what's actually in your wallet on activation. The approval is a promise, not a lock.

**Can I change the beneficiaries later?** Yes — see [Edit or Delete a Legacy Contract](./edit-or-delete-a-legacy-contract.md).

**Does the contract hold custody of my assets?** No (Transfer legacy with EOA or Safe). You keep custody until activation. The contract only has permission to move what you've approved.

## What's next

- [Legacy Contract Details](./legacy-contract-details.md) — what the details view shows, and how to interpret it.
- [Edit or Delete a Legacy Contract](./edit-or-delete-a-legacy-contract.md) — updating allocations, revoking approvals, or tearing everything down.
- [Manage Contingent Beneficiaries](../premium-features/manage-contingent-beneficiaries.md) — Premium fallback layers.
