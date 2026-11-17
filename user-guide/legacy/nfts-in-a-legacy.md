---
description: >-
  Set aside single NFTs for an heir in a wallet legacy of the new version.
  Only collections on the list can be named; on Ethereum the list holds
  three. The NFT stays in your wallet until it is claimed.
---

# NFTs in a Legacy

A wallet legacy of the new version (every wallet legacy created on Ethereum since 2026-11-13 20:42 UTC) can hand on NFTs as well as tokens. You choose the NFT and the heir who receives it. It stays in your wallet until it is claimed, and you can keep using it as before.

It needs no Premium. An older wallet legacy cannot name NFTs; it can [move to the new version](./move-to-the-new-version.md) first.

## Which NFTs can be named

Only NFTs from the collections on 10102's list, ERC-721 and ERC-1155. The list is a contract on Ethereum (`0xF33e887c6BE3bA8Fb583E14164a9e74FD56c1623`), and the legacy's NFT section reads it from the chain, so that section is the place that knows what can be named today.

On Ethereum the list holds three collections since **2026-11-16**, all ERC-721. No ERC-1155 collection is on the Ethereum list today.

| Collection | Contract | What to know |
|---|---|---|
| **CryptoPunks**, in the ERC-721 wrapper "CryptoPunks 721" | `0x000000000000003607fce1aC9e043a86675C5C2F` | Only a punk wrapped in this wrapper can be named. A punk still in the original 2017 contract, or in the older "Wrapped Cryptopunks" wrapper, cannot be named until its owner wraps it in this one. If you unwrap a named punk, your wallet no longer holds the token the legacy names, and nothing moves for that item at claim time. |
| **Bored Ape Yacht Club** | `0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D` | Its contract has no pause, and its ownership was renounced. |
| **Pudgy Penguins** | `0xBd3531dA5CF5857e7CfAA92426877b022e612cf8` | Its contract has an owner key that can pause every transfer. While it is paused, a named penguin cannot move: a claim skips it and it stays owed until the pause ends. It is owed to whoever may take it by then: the heir it was set aside for or, once their turn or period has started, that heir's backup, the backup beneficiary or the second backup. Nobody at 10102 can lift that pause. |

{% hint style="warning" %}
**Being on the list is no endorsement.** It says one thing: when we checked, an NFT of that collection, named in a legacy, could be moved to its heir at claim time. It is not a valuation and not a statement that the collection is safe. The list can change: collections can be added or removed at any time. A collection taken off the list later stays claimable in a legacy that already names it, but it cannot be named again, and saving the NFT list again (or an edit that clears it) drops that item for good. For any other collection, look at the NFT section of your legacy; we do not promise a collection or a date.
{% endhint %}

## Adding NFTs

On the legacy's page, in the **NFTs** section:

1. Press **Add NFTs** (or **Change NFTs** when some are named).
2. For each NFT, choose the **Collection** (shown with its address and its kind), type the **Token ID**, for an ERC-1155 item **How many**, and pick who it **Goes to**: one of your heirs. **Add another** adds a row.
3. Press **Save NFTs**. The app first checks that your wallet holds each NFT you entered and refuses a row it does not hold. Then your wallet asks you to approve each new collection once, and to save the list.

Nothing leaves your wallet. The app confirms: "NFTs saved. They stay in your wallet until claimed."

What to know:

- **Up to 50 NFTs** in one legacy. Saving replaces the whole list.
- **Each NFT goes to one heir.** An NFT cannot be split by shares the way tokens are.
- **The approval.** For each collection your wallet gives one operator, the legacy's vault, an approval for the whole collection (`setApprovalForAll`). Your wallet shows it as access to all your NFTs of that collection. The vault's code moves an NFT only when your own legacy asks, and your legacy asks only for the items you named, after the claim date. That limit is in the contracts' code, which has had no independent audit, not in the approval itself. If a save fails after you approved, the app offers to revoke those approvals.
- **An item that shows "Needs your approval"** has lost its approval; **Approve** next to it gives it again.
- **No claim fee on NFTs.** The claim fee applies to tokens and ETH only.

## What clears the list

Each NFT is tied to one of your heirs. So any change to your heirs or their shares clears the NFT list, and so does adding or removing a backup beneficiary or a second backup, or changing its wait (that save also saves your heirs again). The edit form warns you before such a save, and afterwards the legacy's page tells you to set the list again. See [what an edit resets](./edit-or-delete-a-legacy-contract.md#what-an-edit-resets-on-the-new-version).

## At claim time

From the claim date, an heir claims on the legacy's page or on the heir page. Without backup heirs, one claim by any heir moves every named NFT that can move, each to the heir it was set aside for. With backup heirs on, a claim moves only what the claimant may take in their turn: the heir's own NFTs, or a backup's in the backup's turn. An NFT nobody claimed can also be taken by your backup beneficiary or second backup once their period starts. See [Activate a Legacy Contract and Claim Funds](./activate-a-legacy-contract-and-claim-funds.md#claiming-nfts).

An NFT moves only if, at that moment, your wallet still holds it and the approval for its collection still stands. An item that cannot move (its collection is paused, your wallet no longer holds it, or the approval is gone) is skipped: it stays owed and can be claimed later, once it can move. It is owed to whoever may take it by then: the heir it was set aside for or, once their turn or period has started, that heir's backup, the backup beneficiary or the second backup. An ERC-1155 item can move in parts: what your wallet holds at claim time moves, and the rest stays owed.

Two things follow from a first NFT claim:

- After it, you can no longer change or remove the legacy.
- If an NFT claim ended your legacy, your wallet must release it before it can create a new one. The legacy's page shows a notice with **Release** (one transaction).

## Honest limits

- **Not independently audited.** The new version of the wallet legacy, NFTs included, has had internal adversarial reviews and automated testing and analysis, and no independent audit.
- **We do not control the collections.** A collection's own contract can restrict or pause transfers, and neither you nor 10102 can override it.
- **The NFT must still be there.** Selling, moving or unwrapping a named NFT, or revoking the approval, means nothing moves for that item. The legacy's page shows it: "Not in your wallet right now, so nothing of it can be claimed".
- **Gas.** Approving a collection, saving the list and claiming are ordinary transactions. An heir who claims NFTs pays the network fee from their own wallet.

## See also

- [Legacy: the new version](./README.md#the-new-version-of-the-wallet-legacy)
- [Legacy Contract Details](./legacy-contract-details.md)
- [Backup Beneficiaries and Backup Heirs](../premium-features/manage-contingent-beneficiaries.md)
