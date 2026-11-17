---
description: >-
  A decentralized framework for digital legacy planning: decide what
  happens to your digital assets, without trusting a centralized custodian.
---

# Introduction

Security for digital assets has matured. Smart contracts get audited, wallets get hardware-hardened, users get more discerning. Legacy planning for digital assets has not kept up. Most "solutions" still rely on centralized custodians, legal instruments designed for fiat assets, or a scrap of paper shoved in a safe that nobody remembers where.

**10102 Computing Legacy** is a set of open-source Ethereum smart contracts, with a web app on top, that lets you define the rules for how your on-chain assets pass on, and executes them automatically when those rules are met. No custody. No subscription required for the core flows. No middleman the legacy depends on.

The protocol has been live on Ethereum mainnet since **October 2024** and independently audited twice ([RockSolid Security, report January 2025](https://github.com/10102-io/computing-sc/blob/main/Security_Review_Computing_Will.pdf); [CDSecurity, report October 2025](https://github.com/CDSecurity/audits)). Development started in June 2024; the original contracts repository is [archived and public](https://github.com/10102-labs/computing-sc-og), preserved when the project moved to the `10102-io` GitHub organization in January 2026.

{% hint style="info" %}
**What the two audits do not include.** They were made on the legacy, timelock and Premium contracts as they stood in 2025. What came later had internal adversarial review and automated testing and analysis, not an independent audit. That includes the two newest contracts on mainnet: the new version of the wallet legacy (every wallet legacy created since 2026-11-13: NFTs, backup heirs, an executor) and Shield Vault v3 (every holding opened since 2026-11-11). Each page that describes them says so again.
{% endhint %}

## What it does

- **Transfer legacy**: split specific assets across named Ethereum addresses when you've been inactive for a configurable window. Created from your connected EOA wallet (MetaMask, Ledger, Trezor, Rainbow, Coinbase Wallet, WalletConnect-compatible mobile wallets). A wallet legacy created since 2026-11-13 can also hand on [NFTs from a short list of collections](user-guide/legacy/nfts-in-a-legacy.md) (on the list since 2026-11-16), and an older one can [move to the new version](user-guide/legacy/move-to-the-new-version.md).
- **Multisig legacy**: hand over control of an existing Safe to your beneficiaries by adding them as co-signers when the inactivity window elapses. The Safe itself, and everything it holds or governs, _is_ the legacy.
- **Timelock**: a time-based security layer for your own funds: lock assets until a specific date (protecting against coercion, wrench attacks, or your own impulses). Three flavors: Timelock, Soft Timelock, Beneficiary Timelock.
- **Built on timelocks**: [Save](user-guide/save.md) puts money aside until a date, with a wallet or with just an email; [Heirloom](user-guide/heirloom.md) seals ETH or USDC as a gift that opens on a date, with a claim link.
- **Shield Vault**: [a holding behind a wait](user-guide/shield.md). Every withdrawal waits in public, and a printed recovery sheet can stop one you did not start, if it is used before the wait ends.
- **Wallet alerts**: [an email](user-guide/wallet-alerts.md) when tokens leave your wallet through a permission, when it gives a new one, or when it starts running a contract's code. Free for your own wallet.
- **Premium layer**: optional. A backup beneficiary and a second backup, backups for each heir and an executor on wallet legacies of the new version, authorized watchers (read-only oversight accounts), email reminders, automatic renewal, and gas-free check-ins and withdrawals. All additive; the core flows work without them. See [Premium Features](user-guide/premium-features/README.md).

## What it costs

- **Setting up and keeping** a legacy, a timelock, a gift or a Shield Vault holding carries no 10102 fee. You pay Ethereum network gas.
- **A fee when assets are delivered**: 0.25% today, fixed into each legacy or holding when it is created and shown before you confirm. It is taken when your heirs claim tokens or ETH from a legacy created since 2026-09-29 (older legacies carry none), and when a Shield Vault holding is released to the people you named after your silence (a holding opened before 2026-11-11 keeps the terms it opened with). NFTs handed on through a legacy, timelocks and gifts carry none. Details: [Create a Legacy Contract](user-guide/legacy/create-a-legacy-contract.md) and [Shield Vault](user-guide/shield.md).
- **Premium** is optional: 99 USD per year, or 399 USD once for lifetime, paid on-chain in ETH, USDC or USDT.

## The design principle: your setup survives us

Every legacy, timelock and Shield Vault holding is operable directly from the Ethereum contracts: without our UI, without our servers, without our company. That's not an afterthought; it's the point. The extras that do need our servers or another company (email reminders, wallet alerts, Save on autopilot, the email wallet) say so on their own pages.

- **Contracts are verified on Etherscan**. Anyone can call them from any Ethereum interface.
- **Code and audits are public**. See [github.com/10102-io/computing-sc](https://github.com/10102-io/computing-sc) and [github.com/10102-labs/audits](https://github.com/10102-labs/audits).
- **The Legacy Claim Card is printable**. Every legacy produces a one-pager documenting the contract address, legacy ID, and activation instructions, enough for a beneficiary to claim via Etherscan even decades from now. See [Legacy Claim Card](user-guide/legacy/legacy-claim-card.md).
- **Upgrades are timelocked and publicly visible**. The `DefaultProxyAdmin` for all upgradeable contracts is owned by an on-chain upgrade timelock: no implementation can change without first sitting in a public 48-hour queue that anyone can watch. See [Upgrade Policy](architecture/upgrade-policy.md).

If our website goes down tomorrow, your legacy still works. That's the entire bet.

The terms you accept are public in the same way. The version in force is v2026-12, published at [10102.io/disclosures](https://10102.io/disclosures). Its exact text and hash are at [app.10102.io/terms/current.json](https://app.10102.io/terms/current.json), and the same hash is stored in the verifier contract on Ethereum, so anyone can check the text against the chain.

## Built on Ethereum, with the ecosystem

- **Ethereum**: the settlement layer. Mainnet + Sepolia for testing.
- **Safe**: wallet infrastructure for all multisig-based flows (Safe Guard + Safe Module patterns).
- **The Graph**: subgraphs index legacy contracts, token balances, and activity, so the UI stays fast without trusting any single RPC.
- **Mailjet**: transactional email delivery for the off-chain reminder worker. Email scheduling, encryption, and delivery live entirely off-chain; activation itself never depends on it.

## Wallet support

Any wallet that speaks a standard Ethereum connector works: **MetaMask**, **WalletConnect** (covering most mobile wallets), **Coinbase Wallet** (extension, mobile, and passkey-based smart wallets), **Ledger** + **Trezor** (direct or via Ledger Live / Trezor Suite), **Rainbow**, and others. ENS names render everywhere addresses do.

## Mainnet and Sepolia

You can switch networks inside the app, or open it on the test network with a link ending in `?network=testnet`. **Sepolia** is the public testnet, for legacies and timelocks: free test ETH, and activation periods counted in minutes instead of months, so you can exercise the whole cycle in a short session. The products built on them (the Shield Vault, Stake & Shield, Save, Heirloom and the email wallet) run on mainnet only; see [Networks](user-guide/authentication.md#networks). **Mainnet** is production: real assets, real value, real consequences.

## Where to go next

- New users → start with the [User Guide](user-guide/README.md), in particular [Concepts](user-guide/concepts.md) for the vocabulary.
- Technical readers → [Architecture](architecture/README.md) explains the contracts, the subgraph layer, and the email infrastructure.
- Engineers / contributors → [Design & Engineering Notes](dev/README.md) covers the "why" behind the harder design decisions.
- Agents and integrators → [Agents & Builders](agents-and-builders.md); recommending 10102 to an audience or client book → [Partner Program](partners.md).
- Introducing 10102 to a partner or a desk → [About](about.md), one page with status, links and contact.
