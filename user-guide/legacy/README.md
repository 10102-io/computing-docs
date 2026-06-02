---
description: >-
  Legacy contracts hold the rules for how, when, and to whom your assets pass.
  Two shapes — Transfer and Multisig — for different legacy styles.
---

# Legacy

A **legacy contract** is a small, purpose-built Ethereum contract that sits between you and your beneficiaries. It doesn't take custody of your assets; it just holds the rules — who, how much, and under what condition — and executes them when the condition is met.

There are two shapes to choose from.

## Transfer Legacy

Designed for **asset-level transfer**: "when I've been inactive for X days, beneficiary A gets 40% of these tokens, B gets 60%".

- Works with an **EOA** (MetaMask, Ledger, Trezor, Rainbow, Coinbase Wallet…) or a **Safe**.
- Beneficiaries don't need to work together or sign anything jointly — any one of them can activate once the window has elapsed, and distribution happens according to the pre-set allocations.
- You retain custody until activation. The contract only has the ERC-20 allowances you've explicitly approved.
- Up to 10 primary beneficiaries, each with a percentage allocation summing to 100%.
- Premium users can add up to two additional contingent layers — see [Contingent Beneficiaries](../premium-features/manage-contingent-beneficiaries.md).

When to pick it: you want precise control over what passes to whom, and you don't need your beneficiaries to collectively continue running a Safe.

## Multisig Legacy

Designed for **wallet-level transfer**: your **Safe** _is_ the legacy, and your beneficiaries become its co-signers on activation.

- Requires an existing Safe. Create one at [app.safe.global](https://app.safe.global) first if you don't have one.
- On activation, beneficiaries are added as signers to your Safe. They then control the entire Safe — including anything it holds, stakes, governs, or is credited with (reputation, airdrop eligibility, governance positions, off-chain privileges tied to the address, etc.).
- The activation threshold for the new signers is configurable (e.g. 2-of-3 beneficiaries required to act on the Safe).
- Every on-chain change follows your Safe's existing signature threshold. You're not bypassing your Safe's security model, you're building _on top of_ it.
- Allocations aren't applicable here — the Safe passes as a unit.

When to pick it: your Safe is your long-term vault, you want beneficiaries to take over the whole thing (positions and all), not just cherry-picked assets.

## Transfer vs. Multisig — at a glance

| | Transfer | Multisig |
|---|---|---|
| Wallet type | EOA | Safe only |
| What passes | Specified ERC-20s + storage tokens | The entire Safe wallet |
| Granularity | Per-asset, per-beneficiary percentages | The wallet is one unit |
| Requires beneficiaries to coordinate? | No — any one activates | Yes, once they're co-signers |
| Custody before activation | Owner's wallet | Owner's Safe |

## What's next

- [Create a Legacy Contract](./create-a-legacy-contract.md) — the full creation flow.
- [Legacy Contract Details](./legacy-contract-details.md) — reading and operating a legacy after it's live.
- [Edit or Delete a Legacy Contract](./edit-or-delete-a-legacy-contract.md) — changing the rules or tearing it down.
- [Activate a Legacy Contract and Claim Funds](./activate-a-legacy-contract-and-claim-funds.md) — the beneficiary's side.
- [Legacy Claim Card](./legacy-claim-card.md) — the printable safety net.
