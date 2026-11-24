---
description: >-
  The moving parts of the system and what each one is responsible for. Useful
  as a mental model before diving into specific contracts.
---

# Core Mechanisms

This page is a map of the primitives the app is built on. Each section is short by design: the goal is to give you enough vocabulary to navigate the [Architecture](../architecture/README.md) section with confidence, not to re-explain it.

## Private keys and the custody model

Every Ethereum account, EOA or smart contract, is ultimately secured by one or more private keys. The 10102 app **never handles the owner's private key**: all signing happens in the user's wallet (browser extension, hardware device, Safe co-signing flow). We have no custodial fallback, no password reset, no "sign in with Google" backdoor. This is a feature.

When the app does generate a new key (for a beneficiary who doesn't have an Ethereum address), it happens entirely client-side and the key is delivered to the owner as a QR code on the printable [Legacy Claim Card](../user-guide/legacy/legacy-claim-card.md). The app's backend never sees it. See [New Account Generation for Beneficiaries](../architecture/new-account-generation-for-beneficiaries.md) for the mechanics.

## Smart contracts

Every piece of state that matters long-term (a legacy, a timelock, a premium subscription) lives in a smart contract on Ethereum, not in our backend. Three traits shape how they're designed:

- **Per-user contracts.** Each legacy gets its own contract instance, deployed deterministically via `CREATE2` (see below) at a predictable address. Per-user _storage_ is always isolated: one user's activity can't reach another's slots. For EOA transfer legacies, the on-chain code is shared across users via an [EIP-1167 minimal proxy](https://eips.ethereum.org/EIPS/eip-1167): each clone is a ~45-byte forwarder pointing at one shared implementation, and it keeps that implementation for good. New clones point at legacy v3 since 2026-11-13; older ones at the implementation of their day. Multisig legacies still deploy full per-instance contracts, and timelocks are entries in three shared vault contracts; the EOA path adopted clones to cut creation gas by ~80% on a high-volume flow.
- **Upgradeable routers.** The code that _creates_ legacies (the routers) sits behind a transparent proxy whose admin is owned by a public 48-hour upgrade timelock (see [Upgrade Policy](../architecture/upgrade-policy.md)). We can patch bugs and ship improvements (after a public queue anyone can watch), but cannot silently change the rules of an existing legacy because per-legacy state is in the per-legacy contract, not in the upgradeable router.
- **And contracts with no upgrade path.** A legacy contract, the pull vaults that hold token permissions, the Shield Vaults and the recovery registry cannot be upgraded by anyone. A fix there is a new deployment for new users.
- **Verified.** Every deployment is verified on Etherscan. Addresses and ABIs are published in `contract-addresses.json` of the `computing-sc` repo.
- **Not all equally reviewed.** The contracts were audited twice (reports of January 2025 and October 2025). No published audit report covers anything deployed since October 2025, including every implementation wallet legacies are clones of; [Review status](../architecture/README.md#review-status) lists which is which.

## CREATE2 and predictable addresses

The EVM has two deploy opcodes: `CREATE` (address depends on the deployer nonce, changes over time) and `CREATE2` (address depends on the deployer + a salt + the initcode hash, fully deterministic).

10102 uses `CREATE2` for all per-user contract deployments. Practical consequences:

- **Pre-deploy disclosure.** We can show the owner the address of their legacy contract _before_ they sign the creation transaction. What they sign is what they get.
- **Stable Etherscan history.** Because the initcode hash is the same across all users' legacies of a given type, wallets and explorers can recognize the contract family and drop "suspicious contract" warnings on post-deploy approvals.
- **Auditability.** The address of any legacy can be recomputed from the owner + salt + version. Beneficiaries can verify.

## The router pattern

Routers are the "front door" to each flow:

- `MultisigLegacyRouter` / `TransferEOALegacyRouter` for the two legacy flows (a third router, for Safe-backed Transfer legacies, was retired in May 2026).
- `TimeLockRouter` for all three timelock flavors.
- `PremiumRegistry` for subscriptions; `PremiumSetting` for watcher/reminder configuration.

Each router owns the _deploy-or-update_ logic for its flow. Per-legacy (or per-timelock) contracts are minimal: they hold state, emit events, and are callable by the router. One exception since legacy v3: its additions (NFT items, backup beneficiaries, the executor, the NFT claim) are called on the legacy contract directly, by the owner or the claimant, so they needed no router change. This split lets us:

- Upgrade a router to fix bugs or add features without touching existing per-instance contracts.
- Audit each flow in relative isolation.
- Keep per-instance gas cost low: the EOA transfer flow uses EIP-1167 clones that cost ~1.05M gas per create on mainnet (down from ~6M pre-refactor), and the other flows keep their per-instance bytecode deliberately small.

## Heartbeat and activity tracking

A legacy's activation trigger is an inactivity threshold: "activate if the owner has been idle for N days." The app tracks activity in two fundamentally different ways:

- **Safe owners** have a [Safe Guard](../architecture/legacy-contracts-created-with-safe-sdk.md) installed (the `SafeGuard` contract) that stamps `lastTimestampTxs = block.timestamp` on every Safe transaction. Activity tracking is passive and fully on-chain.
- **EOA owners** use a per-legacy on-chain inactivity timer that resets on heartbeat / edit / withdraw / swap events. Activation is a single on-chain comparison against that timer. Activity that doesn't touch the legacy is invisible to it by default, so EOA owners are expected to check in, unless they opt in to **auto-renew** (Premium), where an attestor observes the wallet's public transaction count and renews the timer for them within strict on-chain bounds (it can only delay activation, never accelerate it). See [Indexing & Activity Tracking](../architecture/indexing-and-activity-tracking.md) and [EOA Activity & Auto-Renew](../architecture/eoa-activity-auto-renew.md).

Both paths have an explicit **Check in** in the UI for owners who want a deliberate heartbeat. For an EOA it is one cheap transaction (~21k gas), and a Premium owner of a wallet legacy can sign it instead and have it sent for free. For a Safe it is a transaction of the Safe that calls `sync()` on the legacy's guard and moves nothing (about 67k gas on a 2 of 3 Safe), with the Safe's own number of signatures; see [Check in](../architecture/legacy-contracts-created-with-safe-sdk.md#check-in).

One optional role can move the date the other way. On a legacy v3 the owner may name an **executor**, who can attest the owner's death once the owner has been silent for half the window; claims then open 30 days later if that is sooner, and any check-in cancels it. Nobody verifies that attestation, so it is only as good as the person named.

## Indexing

The app reads most of its state from subgraphs on [The Graph](https://thegraph.com), which index router events for fast, reliable queries. Subgraphs are eventually consistent (after a transaction mines, there's a brief window before events appear), so the UI falls back to direct on-chain reads via viem for freshness-critical paths like "did my legacy actually get deleted?" See [Indexing & Activity Tracking](../architecture/indexing-and-activity-tracking.md) for the details.

## Premium

Premium is sold on-chain for ETH, USDC, or USDT, $99 for a year or $399 once for a lifetime, and recorded in `PremiumRegistry`. Since 2026-11-07 the registry also knows coupon codes (at most 50% off) and referrer wallets whose share (at most 30%) is paid inside the purchase; see the [Partner Program](../partners.md). While Premium is active, its wallet can configure:

- **Contingent beneficiaries**: fallback layers that activate if primaries don't claim within a configurable window.
- **Backup beneficiaries and an executor** (wallet legacies created since 2026-11-13): up to two backups for each beneficiary's own share, and one address that can bring the claim date forward after a long silence.
- **Authorized watchers**: read-only accounts that can view a legacy but never edit or activate.
- **Email reminders**: opt-in notifications at key lifecycle events, powered by an off-chain reminder worker that keeps recipient emails encrypted off-chain (replacing the retired Chainlink Automation cron + Chainlink Functions email path; see [Email Reminders](../architecture/email-reminders.md)).
- **Automatic renewal, and check-ins and timelock withdrawals without gas**: see [EOA Activity & Auto-Renew](../architecture/eoa-activity-auto-renew.md) and [Gas-Sponsored Intents](../architecture/gas-sponsored-intents.md).

Premium is deliberately orthogonal to the core flows: every legacy works without it, and losing Premium (letting the subscription lapse) doesn't break existing legacies; it just freezes configuration changes on the Premium-gated surfaces until renewal.

## The delivery fee

Creating, keeping, checking in on and changing a legacy carry no 10102 fee. When beneficiaries claim tokens or ETH, a fee is deducted from what they receive: 0.25% today. The rate is copied into each legacy when it is created and never changes for it; new legacies refuse a rate above 0.5%; legacies created before 2026-09-29 carry none; NFTs, timelocks and gifts carry none. The Shield Vault takes its fee only on a release to the named people after the owner's silence. Details: [the delivery fee](../architecture/legacy-contracts-created-with-eoas.md#the-delivery-fee) and [the Shield Vault's fee](../architecture/shield-vault.md#the-fee).

## Storage tokens

ETH can't be granted "approval" the way ERC-20 tokens can. To include native ETH in a Transfer legacy or a Timelock with a uniform approval model, the owner swaps ETH into a **storage token** (WETH, or a liquid staking token like wstETH) via the in-app Uniswap integration, then grants the legacy/timelock approval on that storage token. On activation, the storage token is distributed to beneficiaries; they can unwrap it back to ETH themselves if they want.

This keeps the contract's permission surface uniform (everything is ERC-20) without forcing the contract to custody native ETH.
