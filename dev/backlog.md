---
description: >-
  What we're working on, what we've decided to defer, and the triggers that
  would bring deferred items back onto the active list.
---

# Roadmap

This page is the public-facing summary of work ahead. It's deliberately high-level: specific implementation plans, test matrices, and detailed deferred items live alongside the code in the [`computing-sc`](https://github.com/10102-io/computing-sc) repo.

We publish this because the "plan survives us" principle applies to development too: users and integrators should know what's coming, what's on hold, and why, not just what shipped.

## Active work

Areas we're actively investing in over the next few release cycles:

- **Upgrade-timelock proposer hardening.** Contract upgrades already wait in a public 48-hour queue (see [Upgrade Policy](../architecture/upgrade-policy.md)); the planned next step is moving the proposer role from a single maintainer key to a multisig. Thanks to the timelock, that change will itself be publicly visible when it happens.
- **Asymmetric-permission fix for Multisig legacies.** Today, any Safe owner can edit on-chain fields (beneficiaries, activation trigger, name/note) at Safe threshold, but only the original creator EOA can edit off-chain notification settings (watchers, email reminders). We're designing a `transferCreator` function in `PremiumSetting` to resolve the asymmetry; it needs a proxy upgrade, which we're bundling with the next round of other improvements rather than shipping alone.
- **Audit round 2.** Scheduled follow-up audit covering all router changes since the v1 audit. Reports will be published to [`github.com/10102-labs/audits`](https://github.com/10102-labs/audits) as they land.
- **Better error messages and offline tolerance.** Ongoing: the generic "something went wrong" errors are being replaced with specific, actionable messages. Includes distinguishing subgraph outages, RPC latency, wallet rejections, etc.

## Recently shipped

Formerly on this list, now live on mainnet:

- **Permit2 one-confirmation creates.** Creating a Transfer legacy or timelock is now a single confirmation: one signed Permit2 batch replaces the per-token `approve` transactions, and tokens stay in the owner's wallet until claim. This also delivered the single-prompt create flow we previously tracked under EIP-5792 batching.
- **EIP-1167 minimal-proxy clones for EOA legacies.** New EOA legacies deploy as ~45-byte clones of one audited implementation per network, cutting creation gas by 80%+. Existing legacies are unaffected.
- **EOA activity auto-renew.** Opt-in Premium feature: an attestor observes the owner's public wallet activity and renews the inactivity timer for them, with every safety bound on-chain. See [EOA Activity & Auto-Renew](../architecture/eoa-activity-auto-renew.md).
- **Timelocked upgrades.** `DefaultProxyAdmin` is now owned by an on-chain upgrade timelock: every implementation change waits in a public 48-hour queue before it can execute. See [Upgrade Policy](../architecture/upgrade-policy.md).
- **Gas-sponsored claims.** A beneficiary with no ETH can claim by signing a free EIP-712 authorization; 10102's relayer submits the transaction and pays the gas. The app switches to this path automatically when the beneficiary's wallet can't cover the fee. See [Gas-Sponsored Intents](../architecture/gas-sponsored-intents.md).

## Under evaluation

Ideas we think are promising but haven't committed to:

- **SafeNet integration.** Safe Foundation's forthcoming decentralized transaction-security layer. We've drafted a technical brief ([available on request](mailto:info@10102.io)) covering how our Safe Guard interacts with SafeNet's Transaction Guard contract. Waiting for SafeNet's Phase 2 to stabilize before committing.
- **ERC-8211 smart batching.** Runtime parameter injection and pre/post assertions for multi-transaction flows. Most relevant for the beneficiary-side batch claim path (100+ transfers). Evaluating whether it adds enough over EIP-5792 to justify the complexity.
- **AA wallet integration (EIP-4337).** When AA wallets standardize a way to expose "last outgoing transaction timestamp" on-chain, we can collapse the EOA/Safe activity-tracking distinction. Not a standard yet; tracking the ecosystem.
- **EAS attestations.** Using the Ethereum Attestation Service to let third parties attest to legacy validity (trusted auditor attestations, beneficiary co-testimony for conflict resolution). Schema design is the open question.
- **More beneficiary-onboarding paths.** The current "generate a new address" flow is good for crypto novices but limited. Ideas include passkey-based smart accounts and delegate.xyz-style hot/cold separation for the claim address.

## Deferred

Items we've explicitly decided to _not_ do now, with enough context to pick them up later:

- **Native NFT transfer for Multisig legacies.** NFTs held by the Safe are covered automatically because the whole Safe passes. NFTs as first-class asset types in _Transfer_ legacies are intentionally not supported yet: the allocation semantics ("how do you split a single NFT across 3 beneficiaries?") don't have a clean product answer.
- **Multichain deployment beyond mainnet + Sepolia.** Technically straightforward but substantially increases operational surface. We'd rather do mainnet + Sepolia extremely well than ship on 5 chains with half-working cross-chain UX.

A more granular list, with triggers for when each item becomes urgent, lives in the `computing-sc` repo alongside the code, so `git log` is the audit trail.

## How to influence this

- **Security issues** → `security@10102.io`. We take these seriously and will disclose per responsible-disclosure norms.
- **Feature requests and bug reports** → issues on [`github.com/10102-io/computing-sc`](https://github.com/10102-io/computing-sc/issues).
- **General feedback** → `info@10102.io`.

We don't promise to ship everything that's requested, but we read all of it and it genuinely shapes what moves from "under evaluation" to "active work."
