---
description: >-
  Legacy contracts hold the rules for how, when, and to whom your assets pass.
  Two shapes, Transfer and Multisig, for different legacy styles.
---

# Legacy

A **legacy contract** is a small, purpose-built Ethereum contract that sits between you and your beneficiaries. It doesn't take custody of your assets; it just holds the rules (who, how much, and under what condition) and executes them when the condition is met.

There are two shapes to choose from.

## Transfer Legacy

Designed for **asset-level transfer**: "when I've been inactive for X days, beneficiary A gets 40% of these tokens, B gets 60%".

- Created from your connected wallet, an **EOA** (MetaMask, Ledger, Trezor, Rainbow, Coinbase Wallet…). The app calls it a **wallet legacy**.
- Beneficiaries don't need to work together or sign anything jointly. Any one of them can activate once the window has elapsed, and distribution happens according to the pre-set allocations.
- You retain custody until activation. The contract only has the token permissions you've explicitly given.
- Up to 32 beneficiaries, each with a percentage allocation summing to 100%.
- Premium users can add a backup beneficiary and a second backup and, on the new version, backups for each heir and an executor; see [Backup Beneficiaries and Backup Heirs](../premium-features/manage-contingent-beneficiaries.md) and [Name an Executor](../premium-features/executor.md).
- On the new version, single NFTs from the collections on the list can be named for an heir; see [NFTs in a Legacy](./nfts-in-a-legacy.md).

When to pick it: you want precise control over what passes to whom, and you don't need your beneficiaries to collectively continue running a Safe.

### The new version of the wallet legacy

Every wallet legacy created on Ethereum since **2026-11-13 20:42 UTC** is the new version (the contracts call it legacy v3). You do not choose it: it is what the app creates now. The Sepolia test network has it too, so you can try it with test funds first. Its additions are set on the legacy's own page once it exists, where your beneficiaries are called heirs:

| Addition | What it does | Needs Premium |
|---|---|---|
| **NFTs** | Set aside single NFTs (ERC-721, or an amount of an ERC-1155 id), each for one heir, from the collections on the list. They stay in your wallet until claimed. | No |
| **Backup heirs** | Up to two backups for each heir's own share, each after a wait of 30 to 3,650 days. | Yes |
| **Executor** | Someone you trust who can confirm that you have died, once you have been silent for half your check-in period. Claims then open 30 days after the confirmation, if that is sooner. Any check-in from you cancels it. | Yes |

Heirs and backups also get a page of their own, the **heir page**, where they see their part, when their turn starts, and claim; see [Activate a Legacy Contract and Claim Funds](./activate-a-legacy-contract-and-claim-funds.md#the-heir-page-new-version).

**Older wallet legacies keep working exactly as they are**, with their own code and their own screens. A legacy contract cannot be upgraded, so an older one never gains these additions by itself. Its page shows **A new version is available**, with a guided move: see [Move a Legacy to the New Version](./move-to-the-new-version.md). Safe legacies are not part of this.

### Honest limits of the new version

- **Not independently audited.** The new version has had internal adversarial reviews and automated testing and analysis, and no independent audit. The two firm audits (reports of January 2025 and October 2025) were made on the contracts as they stood then; wallet legacies created since already run code that was changed afterwards with internal review only, and the new version adds more of it. A legacy cannot be upgraded once created: a fix would be a new legacy.
- **Nobody verifies an executor's confirmation.** Neither the contract nor 10102 checks it. Name only someone you trust.
- **Being on the NFT list is no endorsement.** It says nothing about a collection's value or safety, the list can change, and a collection's own contract can restrict or pause transfers.
- **Changes reset what depends on them.** Changing your heirs or their shares turns backup heirs off and clears the NFT list until you set them again. The app warns you first.
- **A move is a new legacy.** A new address, a check-in timer that starts again, and the claim fee of the day of the move.

The terms in force (v2026-12, "Legacy Risks") say the same; read them at [10102.io/disclosures](https://10102.io/disclosures).

## Multisig Legacy

Designed for **wallet-level transfer**: your **Safe** _is_ the legacy, and your beneficiaries become its co-signers on activation.

- Requires an existing Safe. Create one at [app.safe.global](https://app.safe.global) first if you don't have one.
- Each Safe can have one legacy. If you sign for several Safes, you can add a legacy for another one from the Legacy page (**Add a legacy for another Safe**); the Safe check tells you when the Safe you chose already has one.
- On activation, beneficiaries are added as signers to your Safe. They then control the entire Safe, including anything it holds, stakes, governs, or is credited with (reputation, airdrop eligibility, governance positions, off-chain privileges tied to the address, etc.).
- The activation threshold for the new signers is configurable (e.g. 2-of-3 beneficiaries required to act on the Safe).
- Every on-chain change follows your Safe's existing signature threshold. You're not bypassing your Safe's security model, you're building _on top of_ it.
- Allocations aren't applicable here; the Safe passes as a unit.

When to pick it: your Safe is your long-term vault, you want beneficiaries to take over the whole thing (positions and all), not just cherry-picked assets.

## Transfer vs. Multisig at a glance

| | Transfer | Multisig |
|---|---|---|
| Wallet type | EOA | Safe only |
| What passes | Specified ERC-20s + storage tokens; on the new version also NFTs from the list | The entire Safe wallet |
| Granularity | Per-asset, per-beneficiary percentages | The wallet is one unit |
| Requires beneficiaries to coordinate? | No, any one activates | Yes, once they're co-signers |
| Custody before activation | Owner's wallet | Owner's Safe |

## What's next

- [Create a Legacy Contract](./create-a-legacy-contract.md): the full creation flow.
- [Legacy Contract Details](./legacy-contract-details.md): reading and operating a legacy after it's live.
- [Edit or Delete a Legacy Contract](./edit-or-delete-a-legacy-contract.md): changing the rules or tearing it down.
- [NFTs in a Legacy](./nfts-in-a-legacy.md): naming NFTs for an heir, and the list of collections.
- [Move a Legacy to the New Version](./move-to-the-new-version.md): for a wallet legacy created before 2026-11-13 20:42 UTC.
- [Activate a Legacy Contract and Claim Funds](./activate-a-legacy-contract-and-claim-funds.md): the beneficiary's side.
- [Legacy Claim Card](./legacy-claim-card.md): the printable safety net.
