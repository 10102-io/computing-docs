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

- **Legacy v3: NFTs, per-share backup lines, an executor.** Built and running on Sepolia since 2026-11-06: NFTs from an allowlist of collections, each passed to one named beneficiary; a backup line for each beneficiary's share; and an optional executor who can bring the claim date forward after a long silence, which any check-in cancels. It goes to mainnet only after an independent review.
- **The governance Safe to 2-of-3.** The Safe already queues upgrades (see Recently shipped); the next step is a third owner and a 2-of-3 threshold, after which the maintainer key gives up its proposer role.
- **Better error messages and offline tolerance.** Ongoing: the generic "something went wrong" errors are being replaced with specific, actionable messages. Includes distinguishing subgraph outages, RPC latency, wallet rejections, etc.

## Recently shipped

Formerly on this list, now live on mainnet:

- **Partner referral program (phase 1).** Partners share a link
  (`app.10102.io/?via=code`); attribution is recorded only after the
  purchase transaction is verified on-chain, each purchase credits once,
  and every partner gets a scoped stats token to reconcile statements
  independently, including a live `active_subscribers` count read from
  the chain. See the [Partner Program](../partners.md) page. Since the
  2026-11-07 upgrade the Premium registry can also pay a registered
  referrer's share (at most 30%) straight from the purchase and record the
  attribution on-chain (`PlanSubscribedWithCode`), and it supports coupon
  codes (at most 50% off). No referrer and no coupon is configured yet, so
  no purchase is split or discounted today.
- **The 2026-11 contract upgrade.** Live on mainnet since 2026-11-07: new
  wallet legacies use a second pull vault that lets an owner clear the link
  to an ended legacy, and a clone that handles USDT on every payment path
  and refuses a claim fee above 0.5%; the EOA router's code-rotation levers wait in the 48-hour
  upgrade queue. Existing legacies keep the code they were created with.
  It had our own checklist pass and adversarial reviews, not a firm's audit.
- **Upgrades queued by a multisig.** Since 2026-09-24 the governance Safe
  queues upgrades and a separate hardware key is the only one that can
  cancel them; the maintainer key keeps its proposer role until the Safe is
  2-of-3. See [Upgrade Policy](../architecture/upgrade-policy.md).
- **Safe legacies: the Safe manages notifications too.** Since the
  2026-11-07 upgrade a multisig legacy's own Safe may edit its watchers and
  reminder settings, next to the wallet that created it. The app still
  offers these settings to the creator only; doing it from the Safe needs a
  Safe transaction built by hand for now.
- **Audit round 2 (CDSecurity).** The follow-up audit is complete: CDSecurity reviewed the full suite, including the timelocks and premium contracts added since the first audit, with the report published October 2025 in [`github.com/CDSecurity/audits`](https://github.com/CDSecurity/audits) and mirrored in [`github.com/10102-labs/audits`](https://github.com/10102-labs/audits). The original legacy contracts were audited by RockSolid Security ([report January 2025](https://github.com/10102-io/computing-sc/blob/main/Security_Review_Computing_Will.pdf)).
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
- **More beneficiary-onboarding paths.** The current "generate a new address" flow is good for people new to wallets but limited. Ideas include passkey-based smart accounts and delegate.xyz-style hot/cold separation for the claim address.

## Deferred

Items we've explicitly decided to _not_ do now, with enough context to pick them up later:

- **Native NFT transfer for Multisig legacies.** NFTs held by the Safe are covered automatically because the whole Safe passes. For wallet legacies, NFTs arrive with legacy v3 (see Active work): each NFT goes to one named beneficiary rather than being split.
- **Multichain deployment beyond mainnet + Sepolia.** Technically straightforward but substantially increases operational surface. We'd rather do mainnet + Sepolia extremely well than ship on 5 chains with half-working cross-chain UX.

A more granular list, with triggers for when each item becomes urgent, lives in the `computing-sc` repo alongside the code, so `git log` is the audit trail.

## How to influence this

- **Security issues** → `security@10102.io`. We take these seriously and will disclose per responsible-disclosure norms.
- **Feature requests and bug reports** → issues on [`github.com/10102-io/computing-sc`](https://github.com/10102-io/computing-sc/issues).
- **General feedback** → `info@10102.io`.

We don't promise to ship everything that's requested, but we read all of it and it genuinely shapes what moves from "under evaluation" to "active work."
