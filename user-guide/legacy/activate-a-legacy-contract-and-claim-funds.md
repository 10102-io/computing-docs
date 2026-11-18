---
description: >-
  Activate a legacy left to you and claim funds. Works through the 10102 app,
  the Safe platform (for Safe legacies), or directly from any Ethereum
  interface using the printed Legacy Claim Card.
---

# Activate a Legacy Contract and Claim Funds

When a legacy's activation window has elapsed, any of its designated beneficiaries can activate it. This page describes all three paths: via the 10102 app, via the Safe platform (for Safe legacies), and via a blockchain explorer (for any legacy, using the Legacy Claim Card). It also describes the heir page, for wallet legacies of the new version, where heirs and their backups see their turn and claim tokens and NFTs.

## Preflight

Before you can activate:

1. **Enough time must have passed**: the owner must have been inactive (no activity the legacy can see) for at least the configured trigger window.
2. **Your wallet must be one of the configured beneficiaries**: one of the first people named, or a backup once the backup's period has started (a backup beneficiary or second backup of the legacy, or, on the new version, a backup heir of one share).
3. **Gas**: activation is a paid transaction, and the assets are distributed to _all_ beneficiaries according to the allocations, not just to you. For EOA transfer legacies, you usually **don't need any ETH yourself**; see [Claiming without ETH](#claiming-without-eth) below.

## Via the 10102 app

1. Open the app, go to the Legacy page and open **Legacies for Me**. Legacies left to you show up here whenever an address in them matches your connected wallet.
2. Click the legacy to open its details.
3. Click **Check status & claim funds**.
4. If the window has elapsed, you're prompted to sign the activation transaction. Sign and pay gas.
5. The legacy's status moves to **Activated**.

### For Transfer legacies

All approved ERC-20s are distributed from the owner's wallet to the listed beneficiaries according to the allocations, and any native tokens the contract holds are distributed the same way.

**The claim fee.** A legacy created since 2026-09-29 carries a claim fee, fixed when it was created: 0.25% for legacies created today, never more than 0.5%. It is taken from the tokens and ETH as they are delivered, so what arrives is the share minus the fee. Legacies created before that date carry none, and NFTs carry none.

{% hint style="info" %}
**Big legacies distribute in batches.** The contract can process up to 100 transfers per activation transaction. If the legacy you're claiming has more than that (e.g. 4 beneficiaries × 30 tokens = 120 transfers), a **Claim remaining funds** button stays available on the details page until everything is distributed. Any beneficiary can press it, any number of times.
{% endhint %}

### Claiming without ETH

If you're claiming an EOA transfer legacy and your wallet can't pay the network fee (common when the owner generated a fresh wallet for you), the app handles it automatically:

1. Instead of a transaction, your wallet asks for a **free signature** (an EIP-712 message titled *ClaimAuth*). Signing costs nothing.
2. 10102's relay service submits the claim transaction and pays the gas.
3. The app tracks the transaction and shows the result exactly as if you had sent it yourself.

The signature only authorizes the claim you were already entitled to: it names your address, this specific legacy, and the exact asset list, and it can be used once. Nobody, including us, can use it to send funds anywhere other than where the owner's allocations say. If you *do* have ETH, nothing changes: you pay your own fee as before. Owners can disable sponsored claims per legacy on-chain if they prefer; the pay-your-own-gas path always works. Details: [Gas-Sponsored Intents](../../architecture/gas-sponsored-intents.md).

The free path is for the token claim on the legacy's own page. A claim made from the [heir page](#the-heir-page-new-version), and a claim of NFTs, are ordinary transactions that need a little ETH.

### For Multisig legacies

Activation adds the beneficiaries as signers to the owner's Safe, with the threshold the owner configured. From that point on, the Safe is under your collective control: you can act on it in the normal Safe UI, our UI, or any other tool.

## The heir page (new version)

A wallet legacy of the new version (created since 2026-11-13 20:42 UTC) has a public page for the people named in it:

```
app.10102.io/heir/1/<legacy address>
```

The owner's legacy page shows the heir page link once backup heirs are on. Without backup heirs it shows no heir link: the address is `app.10102.io/heir/1/` followed by the legacy's contract address. Connect the wallet the owner named, and the page shows **Your part**:

- Who you are in this legacy: an heir with a share, the first or second backup for someone's share, the backup beneficiary or the second backup of the legacy, or the executor (who receives nothing).
- Where it stands: "Claims open on (date) if the owner has not checked in by then.", "Claims opened on (date).", or that the executor confirmed the owner's death and when claims open.
- What a claim would send you now, token by token ("This claim sends you, after the claim fee:"), with **Claim tokens**. If nothing would move, the page says so instead of offering a button.
- **Shares**, and with backup heirs on, each share's turns with their dates, who has received from it, and what it released so far.
- **NFTs**, each with whom it is for and whether it can move now.

Backup heirs of a single share are not in the indexed list of heirs that **Legacies for Me** is built from, so this page is where they see their turn and claim. Any heir can use it. Only the wallet that was named can receive anything.

A claim made from the heir page is an ordinary transaction from your wallet: it needs a little ETH for the network fee.

### With backup heirs on: each share is claimed on its own

When the owner gave heirs backups, the legacy no longer pays everyone in one claim:

- Each heir claims their own share from the claim date. Nobody waits for anyone else.
- If an heir has not claimed, their first backup can claim that share one wait later, and their second backup two waits later. The page shows each date.
- Whoever receives something from a share first, in their turn, keeps the rest of it, later deposits included. Nobody else can claim from that share afterwards.
- Every heir receives tokens as they are, never converted to ETH.

Without backup heirs, one claim pays every heir their share, as before.

### Claiming NFTs

The **NFTs** section lists the legacy's NFTs, each with whom it is for, and offers **Claim 2 NFTs** (the number one claim would move now). Without backup heirs, one claim by any heir moves every NFT that can move to the heir it was set aside for: yours to your wallet, the others' to theirs. With backup heirs on, a claim moves only what is yours to take in your turn. No claim fee applies to NFTs.

- An NFT can move only if the owner's wallet still holds it and the owner's approval for its collection still stands. One that cannot move right now (its collection is paused, it is not in the owner's wallet, the approval is gone) is skipped and stays owed: it can be claimed later, once it can move. It is owed to whoever may take it by then: the heir it was set aside for or, once their turn or period has started, that heir's backup, the backup beneficiary or the second backup. The page says which it is.
- An ERC-1155 item can move in parts: what the owner's wallet holds now moves, and the rest stays owed.
- If nothing can move yet, the button reads **Nothing to claim yet**.
- After the first claim, the owner can no longer change or remove the legacy.

### When an executor has confirmed the owner's death

The owner of a legacy of the new version can name an [executor](../premium-features/executor.md). If the executor confirms that the owner has died, the claim date moves to 30 days after the confirmation, if that is sooner than it would be. A check-in by the owner cancels it, and so does any change that sends a transaction to the legacy. The owner is emailed only if they created a reminder for this legacy with their own email. Until that date nothing can be claimed; the heir page and the legacy's page show the date.

## Via the Safe platform (Safe legacies only)

Safe-backed legacies can be activated from [app.safe.global](https://app.safe.global) independently of our UI:

1. Open the Safe in app.safe.global.
2. Navigate to the Apps tab and open the 10102 Legacy app (or interact with the module directly, for advanced users).
3. Activate from there. The module does the same work as our UI.

Useful as a backup path if our UI is down, or if you already live in Safe's interface.

## Via a blockchain explorer (any legacy)

This is the path your Legacy Claim Card describes. You can reach it any time, with or without our UI. The [Claim Guide](../../claim-guide.md) walks through it in full, for wallet and Multisig legacies, with what each error means.

1. Open an explorer like Etherscan for the network the legacy is on.
2. Navigate to the **Router contract** from the card. It's a **Transfer Legacy Router** (or EOA Router) for Transfer legacies.
3. Connect your wallet (beneficiary address).
4. On the _Contract → Write_ tab, call the activation function, `activeLegacy`, with the **Legacy ID** from the card, the token addresses to pay out, and `isETH_` (see the note below).
5. Sign and pay gas.

The contract will distribute assets exactly as it would have done from the UI; the router has no knowledge of where the call originated.

{% hint style="warning" %}
**You need the token addresses.** The router's claim function, `activeLegacy`, takes the Legacy ID, the list of token addresses to pay out, and `isETH_` (true to also pay out ETH the legacy contract holds). A call with no tokens and `isETH_` false is refused. A call with no tokens and `isETH_` true activates the legacy and pays out only its ETH. Tokens left out can be claimed with another call. The card lists the tokens that were in the legacy on the day it was made; keep the addresses of any token added since with the card.
{% endhint %}

NFTs of a legacy of the new version are claimed on the legacy contract itself, not on the router: its `claimNfts` function takes the positions of the items in the legacy's NFT list. The heir page does this for you; the explorer path is the fallback.

## After activation

- **Status** reads **Activated** for everyone (owner, beneficiaries, watchers).
- **Watchers** still see the legacy under their watchlist for historical context.
- **Owner actions** are disabled: no more edits, no more deletes. The legacy has done its job.
- **Email notifications** (if configured) go out to impacted parties summarizing the distribution.

## Troubleshooting

- **"Not enough time has passed"**: your clock isn't the authoritative one. The contract checks against the owner's last activity it recorded on-chain (a check-in or a change for a wallet legacy, a Safe transaction for a Safe legacy). Wait, or verify the owner really has been inactive for the trigger window.
- **"You are not a beneficiary"**: the address you're connected with doesn't match a configured beneficiary for this legacy ID. Re-check the card; check the address you expect to be listed; ask the owner if you suspect a typo.
- **"Insufficient gas"**: activation for a legacy with many tokens and beneficiaries can be expensive. For EOA transfer legacies the app normally pays this for you automatically ([Claiming without ETH](#claiming-without-eth)); otherwise coordinate with another beneficiary, or use **Claim remaining funds** to split the cost.
- **"Nothing to claim yet"** (new version): your turn has not started, or nothing that is yours can move right now. The heir page shows the date your turn starts and, for an NFT, why it cannot move.

## See also

- [Legacy Claim Card](./legacy-claim-card.md): the printed safety net.
- [Backup Beneficiaries and Backup Heirs](../premium-features/manage-contingent-beneficiaries.md): who can claim when the first people do not.
- [NFTs in a Legacy](./nfts-in-a-legacy.md): the owner's side of an NFT claim.
- [Concepts: Activation trigger](../concepts.md#activation-trigger).
