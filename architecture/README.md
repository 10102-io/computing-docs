---
description: >-
  How the whole system fits together: smart contracts on Ethereum, subgraphs on
  The Graph, and off-chain services for email and activity tracking.
---

# Architecture

10102 Digital Inheritance is deliberately layered so that the **on-chain layer is self-sufficient**, and the **off-chain layer is strictly additive**. If the off-chain services disappeared tomorrow, every existing legacy and timelock would still function — you'd lose email reminders and the pretty UI, but you'd keep the plan.

## The layers

### 1. On-chain: the core plan

Solidity contracts on Ethereum. Deployed behind upgradeable proxies owned by a Safe multisig. Verified on Etherscan and published at [github.com/10102-io/computing-sc](https://github.com/10102-io/computing-sc).

- **Legacy routers** — `MultisigLegacyRouter`, `TransferLegacyRouter` (Safe owners), `TransferEOALegacyRouter` (plain EOAs).
- **Timelock router** — `TimeLockRouter`.
- **Per-user legacy contracts** — deployed deterministically via `LegacyDeployer` using `CREATE2`, so the address is predictable before deployment.
- **Safe integration** — a Safe Guard tracks last-activity timestamps; a Safe Module executes activation (adding owners or transferring assets).
- **Premium contracts** — `PremiumSetting` stores watcher/reminder configuration; `PremiumRegistry` records subscriptions.

Full address list, for every deployed contract and network, lives in `contract-addresses.json` of the `computing-sc` repository.

### 2. Indexing: The Graph

Multiple subgraphs per chain provide fast, reliable read access to data that would be painful to query from raw RPC:

- **Legacy/timelock subgraph** — indexes creation, edits, deletions, activations, reminder configurations.
- **Token balance subgraph** — indexes ERC-20 / ERC-721 / ERC-1155 balances for wallets interacting with the app.
- **ETH totals subgraph** — periodically aggregates native ETH held under legacy contracts and system-wide totals.

The UI prefers subgraph reads but falls back to direct on-chain reads (via viem) for post-mutation freshness, so stale subgraph data never blocks a newly-valid action.

### 3. Off-chain services

Strictly additive layers that improve UX but can fail without breaking the plan:

- **Chainlink Functions** — bridges Moralis API data into on-chain activation checks for EOA legacies (where the EVM can't natively read a wallet's last-tx timestamp).
- **Chainlink Automation** — daily cron job that drives the email reminder evaluation.
- **Mailjet** — SMTP delivery for reminder emails.
- **Public RPC providers + Etherscan** — fallback read paths the UI can switch to.

## Section index

- [Legacy Contracts Created with Safe SDK](legacy-contracts-created-with-safe-sdk.md) — how Multisig and Safe-backed Transfer legacies work, including the Safe Guard and Safe Module integration.
- [Legacy Contracts Created with EOAs](legacy-contracts-created-with-eoas.md) — how pure-EOA Transfer legacies work, including the approval model and CREATE2 deployment.
- [New Account Generation for Beneficiaries](new-account-generation-for-beneficiaries.md) — client-side keypair generation for beneficiaries without an Ethereum address.
- [Indexing & Activity Tracking](indexing-and-activity-tracking.md) — how The Graph and the hybrid Chainlink/Moralis path feed the app.
- [Email Reminders](email-reminders.md) — the Chainlink Automation + Mailjet workflow for out-of-band notifications.

## A note on upgradeability

Every contract deployed by 10102 sits behind a transparent upgradeable proxy whose admin is a Safe multisig held by the 10102 team. We can patch bugs and ship improvements — we _cannot_ silently drain assets or retroactively alter an existing legacy's terms. Beneficiaries, owners, allocations, and activation windows are stored in per-legacy state that upgrades leave intact. Migration to a community-governed admin is on the public roadmap.
