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

The Legacy page (`app.10102.io/legacy`) opens on one card and one quiet link:

- **A legacy for your wallet**, with **Set up my legacy**: a Transfer legacy, which the app calls a wallet legacy. It splits your assets between named Ethereum addresses when activated. Created from your connected EOA wallet (MetaMask, Ledger, Trezor, …).
- **I use a Safe wallet**: a Multisig legacy, which hands over control of your existing **Safe** to your beneficiaries by adding them as co-signers once activated.

Once you have a legacy, the list keeps **Add a wallet legacy** (while your wallet has none) and **Add a Safe legacy**, or **Add a legacy for another Safe** when a Safe of yours already has one.

Still unsure? Start with the wallet legacy; it's the most common case and doesn't require a Safe. You can always create a Multisig legacy later.

{% hint style="info" %}
**A wallet legacy you create now is the new version** (every wallet legacy created on Ethereum since 2026-11-13 20:42 UTC). Creating it works as this page describes. Once it exists, its page lets you add NFTs and, with Premium, backup heirs and an executor. It is live without an independent audit; see [Legacy: the new version](./README.md#the-new-version-of-the-wallet-legacy).
{% endhint %}

{% hint style="success" %}
If you started from a **Quick action** on the Legacy page, or from a Guardian setup link, we've pre-filled the type and some settings for you. The rest of this page still applies. See [Quick Actions](../quick-actions.md) for the full list.
{% endhint %}

## Create a Transfer legacy

Creating a Transfer legacy is **one transaction**. You configure everything on one screen, confirm once in your wallet, and the contract is live with its asset permissions already in place. Your wallet keeps full custody the whole time; nothing moves until activation.

### Configure

Fill in:

- **Name** (optional): a private label. It is never written to the blockchain, and neither are beneficiary names. It stays on your device, unless you [sign in on each device](../authentication.md#sign-in-on-each-device), which keeps it encrypted on our server for your other devices.
- **Beneficiaries**: up to 32 addresses. If you list only one, we auto-allocate 100%.
- **Allocations**: percentages must sum to 100%.
- **Activation trigger**: inactivity period before beneficiaries can claim, in whole months (1 year is preselected). The form says what counts: "This is a check-in timer. A check-in from your dashboard resets it, and so does any change you make to the legacy. Sending, swapping or using other apps does not, unless you turn on automatic renewal (Premium) once the legacy exists."
- **Assets to include**: the ERC-20 tokens your beneficiaries will be able to claim. You pick them here, in the same form; there is no separate approval step afterwards. A token that reports its own transfers as failed cannot be added, because a legacy could not pass it on; the app checks before any approval and says so. NFTs are not picked here: you add them on the legacy's page once it exists ([NFTs in a Legacy](./nfts-in-a-legacy.md)).
- **Backup beneficiary** and **Second backup** (optional, Premium): one person who can claim once an extra wait has passed, for when your beneficiaries do not, and one more behind them. See [Backup Beneficiaries and Backup Heirs](../premium-features/manage-contingent-beneficiaries.md).

Tick the terms checkbox. Your acceptance is recorded by the create transaction itself; there's no separate "sign this message" prompt. The terms in force are v2026-12, so the box is unticked again for everyone who accepted an earlier version. The link opens [10102.io/disclosures](https://10102.io/disclosures); the exact text and its hash are at [app.10102.io/terms/current.json](https://app.10102.io/terms/current.json), and the verifier contract holds the same hash, so the version you accept can be checked against the chain.

**The claim fee.** Before you create, the page shows the fee taken when your people claim: "Fee: 0.25% of what your people receive, taken when they claim and fixed now for this legacy. Creating it, checking in and changing it cost only network gas." The terms call it the delivery fee. What to know about it:

- It is 0.25% today, and it is fixed into this legacy when you create it. A new legacy can never store more than 0.5%.
- It applies when your people claim tokens or ETH. NFTs handed on through a legacy carry none.
- Legacies created before 2026-09-29 carry no fee, and keep it that way.
- A legacy [moved to the new version](./move-to-the-new-version.md) is a new legacy: it takes the rate of the day of the move, which the move page shows first.
- If the fee cannot be read, the page says so and you cannot create until it can.

{{screenshot: configure-legacy-eoa-step1}}

### Confirm

Click **Create contract**. Two things happen in your wallet:

1. If you included tokens, you sign one **gas-free permission message** covering all of them at once. The permission goes through [Permit2](https://github.com/Uniswap/permit2), the token-permission standard used across the Ethereum ecosystem, and names a single spender: our published, Etherscan-verified vault contract, the same address for every 10102 user. You can check it against the address in [How Legacy Creation Works](../../architecture/create-flow-v2.md).
2. You confirm **one transaction**. It deploys your legacy contract and registers the token permissions together. The contract address is deterministic (`CREATE2`), so the address shown on screen is the address you get.

That's it: the details page opens and your legacy is live.

The create transaction carries the claim fee you were shown. Right before you sign, the app reads the fee again: if it is now higher, nothing is signed and the page asks you to check the new fee. If it rises after you sign, the contract refuses the create and nothing is created. Stake & Shield works the same way when it creates a legacy.

{% hint style="info" %}
**A possible one-time extra.** If your wallet has never used a particular token with Permit2 before (in any app), that token needs a one-time `approve(Permit2)` transaction first. The app detects this and prompts you. It's once per token per wallet, and it's shared with every other Permit2-based app you use.
{% endhint %}

{% hint style="info" %}
**What does the permission actually allow?** Nothing, until activation. Tokens stay in your wallet and remain fully spendable by you. If the legacy activates, beneficiaries receive their configured percentages of whatever is in your wallet at that time. You can revoke the permission at any point by deleting the legacy or directly in your wallet's Permit2 settings.
{% endhint %}

### Adding ETH

ETH can't be permissioned directly because it isn't an ERC-20. On the legacy's edit page, **Add ETH to your legacy** wraps it as **WETH**, the [storage token](../concepts.md#storage-token): 1 WETH for 1 ETH, with no market price involved. The permission for the WETH is set up as part of the same flow. The WETH stays in your wallet until activation.

**Convert back to ETH.** On legacies created since the November 2026 upgrade (2026-11-07 on mainnet), the same card offers **Convert all back to ETH**. It converts the whole balance of the stored token in your wallet, never part of it: once any of it is converted, the legacy stops treating the rest as ETH you set aside. WETH turns back into ETH 1 to 1. A legacy that keeps its ETH as another token converts it at the current price, and if the price falls more than 2% before it confirms, nothing is converted. The ETH lands in your wallet, where it is no longer in your legacy. Converting also counts as a check-in. Older legacies cannot do this: swap or unwrap the token in your wallet instead.

### Print a Legacy Claim Card (optional but recommended)

The app generates a one-page printable card with the minimum information your beneficiaries need to claim, even if our UI is ever unavailable. See [Legacy Claim Card](./legacy-claim-card.md).

## Safe-based legacies (Multisig / Safe Transfer)

Safe-based flows work differently from the EOA flow above: creation goes through your Safe's own transaction process, so your Safe's co-signers approve it at your normal threshold.

### Create a Multisig legacy

Multisig legacy _is_ your Safe: beneficiaries become co-signers on activation, taking over the wallet and any positions it holds elsewhere (staking, governance, NFTs, etc.).

1. On the Legacy page, click **I use a Safe wallet** (or **Add a Safe legacy** once you have a legacy).
2. Enter your Safe address. We verify it, and the page says what is wrong when it cannot be used: not an address, not a Safe on this network, already has a legacy, or could not be checked. Each Safe can have one legacy; a wallet that signs for several Safes can give each its own.
3. Configure:
   - Contract name, beneficiaries (addresses), activation trigger.
   - How many signatures the Safe needs after activation. As you add beneficiaries the form says what that comes to, for example "After activation: 1 of 5 signatures required": your beneficiaries join the Safe's current signers, and the number counts for all of them.
   - No asset allocation here; beneficiaries take over the Safe itself.
4. Your Safe signers approve the creation transaction (installs our Safe module and guard).

{{screenshot: configure-multisig-check-safe}}

{% hint style="warning" %}
The Safe must still be in active use, because Multisig legacy relies on your Safe's last outgoing transaction as a liveness signal. If you don't transact from the Safe at least once per activation window, beneficiaries can activate. If you have no other transaction to send, use **Check in** on the details page: it restarts the waiting period without moving anything, and like every transaction of the Safe it needs the Safe's usual number of signers.
{% endhint %}

## After it exists: NFTs, backups and an executor

A wallet legacy of the new version has three more sections on its own page. None of them is part of the create form:

- **NFTs**: name single NFTs for an heir, from the collections on the list. See [NFTs in a Legacy](./nfts-in-a-legacy.md).
- **Backup heirs** (Premium): up to two backups for each heir's own share. See [Backup Beneficiaries and Backup Heirs](../premium-features/manage-contingent-beneficiaries.md#backup-heirs-for-each-share).
- **Executor** (Premium): someone who can confirm that you have died. See [Name an Executor](../premium-features/executor.md).

## Premium: a backup beneficiary and a second backup

If you subscribe to Premium, the create form lets you name a backup beneficiary and, behind them, a second backup: fallbacks if your beneficiaries never claim (lost keys, deceased, etc.). See [Backup Beneficiaries and Backup Heirs](../premium-features/manage-contingent-beneficiaries.md).

## Common questions

**Does the contract hold custody of my assets?** No, for Transfer legacy. Your tokens never leave your wallet before activation. The create transaction only grants a claim permission, and only your own legacy contract can ever use it.

**What happens if I move my included tokens?** Nothing breaks. The permission is a promise, not a lock: beneficiaries can only claim what's actually in your wallet on activation. Spend, stake, or move your tokens as usual.

**Can I add ETH directly to the legacy?** Not for Transfer legacies; ETH must be wrapped as WETH first. This is by design: the legacy contract works through ERC-20 token permissions, which ETH doesn't have.

**Can I include more tokens later?** Yes: on the legacy's edit page, under **Tokens for your legacy**, press **Approve** next to the token. It gives the same standing permission a ticked token gets at creation (Permit2, to the legacy's vault), with no amount to pick or raise later; a token your wallet holds in full in the legacy reads **All in your legacy**. On an older legacy, created before standing permissions, **Approve** sets an amount instead, so approve again when your balance grows.

**Can I include NFTs?** On a wallet legacy of the new version, yes, from the collections on the list, once the legacy exists. See [NFTs in a Legacy](./nfts-in-a-legacy.md).

**Where are the names stored?** The legacy's name and your beneficiaries' names live on your device, not on the blockchain. On-chain there are only addresses, percentages, and the trigger; see [How Legacy Creation Works](../../architecture/create-flow-v2.md). If you [sign in on each device](../authentication.md#sign-in-on-each-device), the names also follow your wallet to your other devices: stored encrypted on our server and shown only to your wallet, signed in. Notes stay on the device.

**Can I change the beneficiaries later?** Yes; see [Edit or Delete a Legacy Contract](./edit-or-delete-a-legacy-contract.md).

## What's next

- [Legacy Contract Details](./legacy-contract-details.md): what the details view shows, and how to interpret it.
- [Edit or Delete a Legacy Contract](./edit-or-delete-a-legacy-contract.md): updating allocations, revoking permissions, or tearing everything down.
- [Backup Beneficiaries and Backup Heirs](../premium-features/manage-contingent-beneficiaries.md): Premium fallback layers.
- [NFTs in a Legacy](./nfts-in-a-legacy.md) and [Name an Executor](../premium-features/executor.md): what the new version adds.
- [How Legacy Creation Works](../../architecture/create-flow-v2.md): the architecture behind the one-transaction create, for the curious.
