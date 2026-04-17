---
description: >-
  The moving parts of the system and what each one is responsible for — useful
  as a mental model before diving into specific contracts.
---

# Core Mechanisms

This page is a map of the primitives the app is built on. Each section is short by design: the goal is to give you enough vocabulary to navigate the [Architecture](../architecture/README.md) section with confidence, not to re-explain it.

## Private keys and the custody model

Every Ethereum account — EOA or smart contract — is ultimately secured by one or more private keys. The 10102 app **never handles the owner's private key**: all signing happens in the user's wallet (browser extension, hardware device, Safe co-signing flow). We have no custodial fallback, no password reset, no "sign in with Google" backdoor. This is a feature.

When the app does generate a new key — for a beneficiary who doesn't have an Ethereum address — it happens entirely client-side and the key is delivered to the owner as a QR code on the printable [Legacy Claim Card](../user-guide/legacy/legacy-claim-card.md). The app's backend never sees it. See [New Account Generation for Beneficiaries](../architecture/new-account-generation-for-beneficiaries.md) for the mechanics.

## Smart contracts

Every piece of state that matters long-term (a legacy, a timelock, a premium subscription) lives in a smart contract on Ethereum, not in our backend. Three traits shape how they're designed:

- **Per-user contracts.** Each legacy or timelock gets its own contract instance, deployed deterministically via `CREATE2` (see below). This keeps state isolated — a bug in one user's legacy can't corrupt another's — and lets the contract address itself be printed on the Legacy Claim Card as a stable reference.
- **Upgradeable routers.** The code that _creates_ legacies (the routers) sits behind a transparent proxy whose admin is a Safe multisig. We can patch bugs and ship improvements, but cannot silently change the rules of an existing legacy because per-legacy state is in the per-legacy contract, not in the upgradeable router.
- **Verified.** Every deployment is verified on Etherscan. Addresses and ABIs are published in `contract-addresses.json` of the `computing-sc` repo.

## CREATE2 and predictable addresses

The EVM has two deploy opcodes: `CREATE` (address depends on the deployer nonce, changes over time) and `CREATE2` (address depends on the deployer + a salt + the initcode hash, fully deterministic).

10102 uses `CREATE2` for all per-user contract deployments. Practical consequences:

- **Pre-deploy disclosure.** We can show the owner the address of their legacy contract _before_ they sign the creation transaction. What they sign is what they get.
- **Stable Etherscan history.** Because the initcode hash is the same across all users' legacies of a given type, wallets and explorers can recognize the contract family and drop "suspicious contract" warnings on post-deploy approvals.
- **Auditability.** The address of any legacy can be recomputed from the owner + salt + version. Beneficiaries can verify.

## The router pattern

Routers are the "front door" to each flow:

- `MultisigLegacyRouter` / `TransferLegacyRouter` / `TransferEOALegacyRouter` for the three legacy flows.
- `TimeLockRouter` for all three timelock flavors.
- `PremiumRegistry` for subscriptions; `PremiumSetting` for watcher/reminder configuration.

Each router owns the _deploy-or-update_ logic for its flow. Per-legacy (or per-timelock) contracts are minimal — they hold state, emit events, and are callable by the router. This split lets us:

- Upgrade a router to fix bugs or add features without touching the per-instance contracts.
- Audit each flow in relative isolation.
- Keep per-instance gas cost low (less code per deploy).

## Heartbeat and activity tracking

A legacy's activation trigger is an inactivity threshold — "activate if the owner has been idle for N days." The app tracks activity in two fundamentally different ways:

- **Safe owners** have a [Safe Guard](../architecture/legacy-contracts-created-with-safe-sdk.md) installed that captures `lastOutgoingTxTimestamp` on every Safe transaction. Activity tracking is passive and fully on-chain.
- **EOA owners** use a per-legacy counter that resets on heartbeat / edit events, plus a [Chainlink + Moralis hybrid](../architecture/indexing-and-activity-tracking.md) at activation time to verify the EOA hasn't transacted elsewhere.

Both paths surface an explicit "I'm still alive" button in the UI for owners who want a deliberate heartbeat. The button is cheap (~21k gas for EOAs) and unambiguous.

## Indexing

The app reads most of its state from subgraphs on [The Graph](https://thegraph.com), which index router events for fast, reliable queries. Subgraphs are eventually-consistent — after a transaction mines, there's a brief window before events appear — so the UI falls back to direct on-chain reads via viem for freshness-critical paths like "did my legacy actually get deleted?" See [Indexing & Activity Tracking](../architecture/indexing-and-activity-tracking.md) for the details.

## Premium

Premium is a time-bound subscription sold for ETH, USDC, or USDT, recorded on-chain in `PremiumRegistry`. While a subscription is active, its wallet can configure:

- **Contingent beneficiaries** — fallback layers that activate if primaries don't claim within a configurable window.
- **Authorized watchers** — read-only accounts that can view a legacy but never edit or activate.
- **Email reminders** — opt-in notifications at key lifecycle events, powered by Chainlink Automation + Mailjet (see [Email Reminders](../architecture/email-reminders.md)).

Premium is deliberately orthogonal to the core flows: every legacy works without it, and losing Premium (letting the subscription lapse) doesn't break existing legacies — it just freezes configuration changes on the Premium-gated surfaces until renewal.

## Storage tokens

ETH can't be granted "approval" the way ERC-20 tokens can. To include native ETH in a Transfer legacy or a Timelock with a uniform approval model, the owner swaps ETH into a **storage token** — WETH, or a liquid staking token like wstETH — via the in-app Uniswap integration, then grants the legacy/timelock approval on that storage token. On activation, the storage token is distributed to beneficiaries; they can unwrap it back to ETH themselves if they want.

This keeps the contract's permission surface uniform (everything is ERC-20) without forcing the contract to custody native ETH.
