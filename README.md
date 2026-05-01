---
description: >-
  A decentralized framework for digital inheritance — give your crypto a
  survivable plan, without trusting a centralized custodian.
---

# Introduction

Crypto security has matured. Smart contracts get audited, wallets get hardware-hardened, users get more discerning. Inheritance planning for crypto has not kept up. Most "solutions" still rely on centralized custodians, legal instruments designed for fiat assets, or a scrap of paper shoved in a safe that nobody remembers where.

**10102 Computing Legacy** is a small, auditable set of Ethereum smart contracts — plus a thin app on top — that lets you define the rules for how your on-chain assets pass on, and executes them automatically when those rules are met. No custody. No subscription required for the core flows. No middleman the plan depends on.

## What it does

- **Transfer legacy** — split specific assets across named Ethereum addresses when you've been inactive for a configurable window. Created from your connected EOA wallet (MetaMask, Ledger, Trezor, Rainbow, Coinbase Wallet, WalletConnect-compatible mobile wallets).
- **Multisig legacy** — hand over control of an existing Safe to your beneficiaries by adding them as co-signers when the inactivity window elapses. The Safe itself, and everything it holds or governs, _is_ the inheritance.
- **Timelock** — a time-based security layer for your own funds: lock assets until a specific date (protecting against coercion, wrench attacks, or your own impulses). Three flavors: Timelock, Soft Timelock, Timelocked Gift.
- **Premium layer** — optional contingent beneficiaries (fallback layers), authorized watchers (read-only oversight accounts), and email reminders. All additive; the core flows work without them.

## The design principle: your plan survives us

Every feature in this app is operable directly from the Ethereum contracts — without our UI, without our servers, without our company. That's not an afterthought; it's the point.

- **Contracts are verified on Etherscan**. Anyone can call them from any Ethereum interface.
- **Code and audits are public**. See [github.com/10102-io/computing-sc](https://github.com/10102-io/computing-sc) and [github.com/10102-labs/audits](https://github.com/10102-labs/audits).
- **The Legacy Claim Card is printable**. Every legacy produces a one-pager documenting the contract address, legacy ID, and activation instructions — enough for a beneficiary to claim via Etherscan even decades from now. See [Legacy Claim Card](user-guide/legacy/legacy-claim-card.md).
- **Upgrades are Safe-controlled, not founder-controlled**. The `DefaultProxyAdmin` for all upgradeable contracts is owned by a multisig.

If our website goes down tomorrow, your legacy still works. That's the entire bet.

## Built on Ethereum, with the ecosystem

- **Ethereum** — the settlement layer. Mainnet + Sepolia for testing.
- **Safe** — wallet infrastructure for all multisig-based flows (Safe Guard + Safe Module patterns).
- **The Graph** — subgraphs index legacy contracts, token balances, and activity, so the UI stays fast without trusting any single RPC.
- **Chainlink** — Functions + Automation drive the hybrid on-chain/off-chain activation check and the daily email reminder cron.

## Wallet support

Any wallet that speaks a standard Ethereum connector works: **MetaMask**, **WalletConnect** (covering most mobile wallets), **Coinbase Wallet** (extension, mobile, and passkey-based smart wallets), **Ledger** + **Trezor** (direct or via Ledger Live / Trezor Suite), **Rainbow**, and others. ENS names render everywhere addresses do.

## Mainnet and Sepolia

You can switch networks inside the app. **Sepolia** is the public testnet: identical flow, free test ETH, activation windows configurable in minutes instead of days so you can exercise the whole cycle in a short session. **Mainnet** is production — real assets, real value, real consequences.

## Where to go next

- New users → start with the [User Guide](user-guide/README.md), in particular [Concepts](user-guide/concepts.md) for the vocabulary.
- Technical readers → [Architecture](architecture/README.md) explains the contracts, the subgraph layer, and the email infrastructure.
- Engineers / contributors → [Design & Engineering Notes](dev/README.md) covers the "why" behind the harder design decisions.
