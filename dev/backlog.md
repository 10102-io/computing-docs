---
description: >-
  What we're working on, what we've decided to defer, and the triggers that
  would bring deferred items back onto the active list.
---

# Roadmap

This page is the public-facing summary of work ahead. It's deliberately high-level — specific implementation plans, test matrices, and detailed deferred items live alongside the code in the [`computing-sc`](https://github.com/10102-io/computing-sc) repo.

We publish this because the "plan survives us" principle applies to development too: users and integrators should know what's coming, what's on hold, and why — not just what shipped.

## Active work

Areas we're actively investing in over the next few release cycles:

- **EIP-5792 atomic batching (`wallet_sendCalls`).** Collapsing the two-step "deploy legacy + approve tokens" flow into a single user prompt on wallets that support the spec, while keeping the explicit two-transaction fallback for wallets that don't. Biggest UX win in the EOA flow. Tracked against MetaMask, Coinbase Wallet, and Rabby rollout timelines.
- **Permit2 integration for Transfer and Timelock flows.** Replacing per-token `approve` transactions with typed-data signatures for short-lived authorizations. Reduces the number of wallet interactions and the "suspicious approval" warnings that wallets surface for freshly-deployed contracts.
- **Asymmetric-permission fix for Multisig legacies.** Today, any Safe owner can edit on-chain fields (beneficiaries, activation trigger, name/note) at Safe threshold, but only the original creator EOA can edit off-chain notification settings (watchers, email reminders). We're designing a `transferCreator` function in `PremiumSetting` to resolve the asymmetry — needs a proxy upgrade, which we're bundling with the next round of other improvements rather than shipping alone.
- **Audit round 2.** Scheduled follow-up audit covering all router changes since the v1 audit. Reports will be published to [`github.com/10102-labs/audits`](https://github.com/10102-labs/audits) as they land.
- **Better error messages and offline tolerance.** Ongoing — the generic "something went wrong" errors are being replaced with specific, actionable messages. Includes distinguishing subgraph outages, RPC latency, Chainlink subscription exhaustion, etc.

## Under evaluation

Ideas we think are promising but haven't committed to:

- **SafeNet integration.** Safe Foundation's forthcoming decentralized transaction-security layer. We've drafted a technical brief ([available on request](mailto:info@10102.io)) covering how our Safe Guard interacts with SafeNet's Transaction Guard contract. Waiting for SafeNet's Phase 2 to stabilize before committing.
- **ERC-8211 smart batching.** Runtime parameter injection and pre/post assertions for multi-transaction flows. Most relevant for the beneficiary-side batch claim path (100+ transfers). Evaluating whether it adds enough over EIP-5792 to justify the complexity.
- **AA wallet integration (EIP-4337).** When AA wallets standardize a way to expose "last outgoing transaction timestamp" on-chain, we can collapse the EOA/Safe activity-tracking distinction. Not a standard yet; tracking the ecosystem.
- **EAS attestations.** Using the Ethereum Attestation Service to let third parties attest to legacy validity (trusted auditor attestations, beneficiary co-testimony for conflict resolution). Schema design is the open question.
- **More beneficiary-onboarding paths.** The current "generate a new address" flow is good for crypto novices but limited. Ideas include passkey-based smart accounts and delegate.xyz-style hot/cold separation for the claim address.

## Deferred

Items we've explicitly decided to _not_ do now, with enough context to pick them up later:

- **Migration of `DefaultProxyAdmin` to community governance.** Today the proxy admin is owned by a 10102-team Safe. Migration to a token-gated or DAO-style governance surface is on the roadmap, but gated on there being meaningful off-team participation to migrate to.
- **Native NFT transfer for Multisig legacies.** NFTs held by the Safe are covered automatically because the whole Safe passes. NFTs as first-class asset types in _Transfer_ legacies are intentionally not supported yet — the allocation semantics ("how do you split a single NFT across 3 beneficiaries?") don't have a clean product answer.
- **Multichain deployment beyond mainnet + Sepolia.** Technically straightforward but substantially increases operational surface. We'd rather do mainnet + Sepolia extremely well than ship on 5 chains with half-working cross-chain UX.

A more granular list — with triggers for when each item becomes urgent — lives in the `computing-sc` repo alongside the code, so `git log` is the audit trail.

## How to influence this

- **Security issues** → `security@10102.io`. We take these seriously and will disclose per responsible-disclosure norms.
- **Feature requests and bug reports** → issues on [`github.com/10102-io/computing-sc`](https://github.com/10102-io/computing-sc/issues).
- **General feedback** → `info@10102.io`.

We don't promise to ship everything that's requested, but we read all of it and it genuinely shapes what moves from "under evaluation" to "active work."
