---
description: >-
  A time-based security layer for self-custody wallets. Lock your own funds
  against coercion, wrench attacks, or just your own impulse decisions.
---

# Timelock

A **Timelock** is a contract that holds your assets in a frozen state for a period of time. Nothing can move the locked assets until the lock expires: not your own keys, not a compromised signer, not a thief holding a physical wrench.

Timelocks are unrelated to legacies. They don't need beneficiaries and they don't distribute on activation. Their job is simpler: _prevent movement for a while, then return the funds to you_ (or, in one variant, deliver them to a designated recipient).

Three flavors, for three different use cases:

## Timelock

**Locked until a specific date; then the owner reclaims.**

- Pick a duration up front. The contract releases on that date, no exceptions, no early withdrawal.
- Useful for: self-imposed HODL periods, pre-commitment devices, cooling-off periods, protecting against your own impulse trading.

## Soft Timelock

**Locked indefinitely; unlocking triggers a configurable waiting period.**

- Locked assets stay locked until you explicitly initiate an unlock, which starts a waiting clock (days). Only after the clock runs out can you claim.
- This is the anti-wrench-attack variant: even with full access to your keys, an attacker can't immediately drain the balance. They have to initiate an unlock and wait, during which you (or anyone watching) can notice.
- Useful for: high-value cold-storage-style protection without cold-storage operational overhead, compromise-tolerant self-custody.

## Beneficiary Timelock

**Locked until a specific date; then a designated recipient claims.**

- Same release mechanics as a regular Timelock, but the recipient is another wallet, not the owner. (The contract calls this type `Gift` on-chain; the app labels it Beneficiary timelock.)
- Useful for: planned giving, trust-fund-style disbursements, escrow-like arrangements.
- For an actual gift-giving experience (occasion designs, a claim link the recipient opens, printable sheets, free claiming), use [Heirloom](../heirloom.md), which seals the same contract type with a friendlier wrapper.

## Which to pick

| You want to | Use |
|---|---|
| Force yourself not to touch funds for N days | **Timelock** |
| Protect yourself if someone coerces you or compromises your keys | **Soft Timelock** |
| Schedule a future transfer to someone else | **Beneficiary Timelock** |
| Send someone a sealed gift they open with a link | [**Heirloom**](../heirloom.md) |

## How timelocks hold your assets

- **ERC-20 tokens**: approved to the timelock contract, moved in when you finalize creation. Once in the contract, they can't leave until the lock expires (or, for Soft Timelock, until the waiting period after unlock elapses).
- **ETH**: can't be held directly (ETH isn't an ERC-20 and can't be approved as one). Instead, you swap ETH into a supported **storage token** (WETH or a liquid staking token) through a built-in swap route, then the timelock holds that token. When the lock expires, you get the storage token back; you can swap it back to ETH at your discretion.
- **NFTs and ERC-1155**: not offered in the app's timelock form. Timelocks are focused on fungible asset protection. To hand on an NFT, see [NFTs in a Legacy](../legacy/nfts-in-a-legacy.md).
- **Fees**: timelocks and gifts carry no delivery fee. You pay network gas.
- **Test network and Cypherpunk Mode**: timelocks are one of the two foundations, so they work on the Sepolia test network and in [Cypherpunk Mode](../hide-amounts-and-cypherpunk-mode.md#cypherpunk-mode). The products built on them (Save, Heirloom) are mainnet products; Heirloom stays open in Cypherpunk Mode.

See [Using Timelock](./using-timelock.md) for the full create/claim walkthrough.
