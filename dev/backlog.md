---
description: >-
  What we're working on, what we've decided to defer, and the triggers that
  would bring deferred items back onto the active list.
---

# Roadmap

This page is the public-facing summary of work ahead. It's deliberately high-level: specific implementation plans, test matrices, and detailed deferred items live alongside the code: for the contracts in the `docs/` folder of the public [`computing-sc`](https://github.com/10102-io/computing-sc) repository, for the app in its own repository, which is not public.

We publish this because the "your setup survives us" principle applies to development too: users and integrators should know what's coming, what's on hold, and why, not just what shipped.

Last reviewed against what is live on mainnet: 2026-11-16.

## Active work

- **The governance Safe to 2-of-3.** The Safe already queues upgrades (see Recently shipped), with two owners and a threshold of one. Next: a third owner and a 2-of-3 threshold, after which the maintainer key gives up its proposer role and the upgrade delay rises from 48 hours to 7 days. None of the three is done.
- **Finishing the app side of legacy v3.** The contracts are live (see Recently shipped). Still open in the app: the older claim card does not yet show a first-line beneficiary's own share as claimed on a legacy with backup beneficiaries, the printed claim card's wording does not yet describe such a legacy, and the fee line does not yet say that on a legacy v3 the fee reaches the treasury as the claimed token. Notification settings for a Safe legacy can be edited by the Safe on-chain, with no screen for it yet.
- **Clear signing for the new functions.** Descriptors for the create that binds the fee and for Shield Vault v3 are submitted to the clear-signing registry and wait for its review. The legacy v3 settings and claims, the vault's `unbind`, the coupon purchase and the exact-date lock have none yet.
- **Better error messages and offline tolerance.** Ongoing: the generic "something went wrong" errors are being replaced with specific, actionable messages. Every legacy v3 contract error has plain words since 2026-11-11.

## Queued for the next contract upgrade

Nothing here is scheduled; an upgrade is assembled when there is enough to put in it, and each one waits 48 hours in the public queue.

- **Compiler.** The next contracts we deploy are to be compiled with solc 0.8.38 or a later release that has had time to settle. Nothing live is redeployed for it: no known compiler bug is found to apply to what is on mainnet.
- **Removing the first-generation create.** The app no longer calls the router's original `createLegacy`; it can leave the router.

## Recently shipped

Formerly on this list, now live on mainnet:

- **Safe legacies: a waiting period only the Safe restarts.** Since 2026-11-21 23:05 UTC a Safe legacy created on Ethereum gets a guard whose waiting period restarts only with the Safe's own transactions. Before, a call from another address could restart it too, which could delay an activation and never bring one forward or move anything. The router now also activates a Safe legacy only while it is connected to its Safe. A legacy created earlier keeps its guard until its owners delete it and create it again; the app says so on its page. The change waited 48 hours in the public queue and was [executed](https://etherscan.io/tx/0xd0eac00d1266de13751bc5523631245ba33b24d2dddf649516e8b6751003acf1) as two new implementations. It had internal review, tests on the Safe's own contracts and rehearsals on a fork of mainnet, and no independent audit. See [Guard versions](../architecture/legacy-contracts-created-with-safe-sdk.md#guard-versions).
- **The router's create flag.** Since 2026-11-18 23:55 UTC only the claim that ends a legacy releases the router's "this wallet has a legacy" flag. Before, a repeat claim on a legacy that had already ended cleared it, even if the owner had created a new one since. No funds moved differently: the pull vault refuses a second create while a live legacy is bound in it, and the app has checked the vault since 2026-11-11. The fix waited 48 hours in the public queue and was [executed](https://etherscan.io/tx/0xc73e70f93160c030d4c21c15d5e3f1e5e919602493683ea5f38f9b5d072e84cd) as one new router implementation ([`0x4251A1EC6D88995d82D7b4397ef74867A8d3F8DC`](https://etherscan.io/address/0x4251A1EC6D88995d82D7b4397ef74867A8d3F8DC#code)). It changed the router alone; legacies, the vaults and every address stay as they are, and a claim, a check-in, a delete or a withdrawal works as before. It had internal review, tests and rehearsals on a fork of mainnet, and no independent audit. A flag that was cleared wrongly before the upgrade stays cleared, so the app keeps its check.
- **Legacy v3: NFTs, backup beneficiaries, an executor.** New wallet legacies are legacy v3 since 2026-11-13 20:42 UTC: single NFTs from a list of collections, each for one named beneficiary; up to two backups for each beneficiary's own share, after a wait of 30 to 3,650 days; and an optional executor who can bring the claim date forward to 30 days out after the owner has been silent for half their period, which any check-in cancels. Backups and the executor are part of Premium. The NFT list holds three collections on mainnet since 2026-11-16: CryptoPunks (in the ERC-721 wrapper "CryptoPunks 721"), Bored Ape Yacht Club and Pudgy Penguins. Legacies created before keep their code; the app offers a guided move, which is a delete and a new create. **It went live before an external review** (see Deferred). See [Legacy Contracts Created with EOAs](../architecture/legacy-contracts-created-with-eoas.md#legacy-v3).
- **Shield Vault v3.** Since 2026-11-11 new holdings open in a new vault whose fee is taken only when a holding is released to the named people after the owner's silence, never on the owner's own withdrawals. Holdings in the earlier vault keep every exit and the terms they opened with; its new deposits are paused. Also live before an external review. See [Shield Vault](../architecture/shield-vault.md).
- **Terms that can be checked against the chain.** The terms in force (`v2026-12`, since 2026-11-16) are published with their hash, and that hash is the one the terms contract holds. They state the legacy v3 risks and the delivery fee as the contracts charge it.
- **Gas-free check-ins and timelock withdrawals** for Premium owners, since 2026-11-09: a free signature, 10102 pays the network fee, and a normal transaction is the fallback. See [Gas-Sponsored Intents](../architecture/gas-sponsored-intents.md).
- **Coupon codes and on-chain partner shares.** Since the 2026-11-07 upgrade the Premium registry supports coupon codes (at most 50% off) and can pay a registered referrer's share (at most 30%) straight from the purchase, recording the attribution on-chain (`PlanSubscribedWithCode`); the checkout has a coupon field since 2026-11-09. Coupons and referrer shares are set up partner by partner. See the [Partner Program](../partners.md).
- **Partner referral program.** Partners share a link
  (`app.10102.io/?via=code`); attribution is recorded only after the
  purchase transaction is verified on-chain, each purchase credits once,
  and every partner gets a scoped stats token to reconcile statements
  independently, including a live `active_subscribers` count read from
  the chain.
- **The 2026-11 contract upgrade.** Live on mainnet since 2026-11-07: new
  wallet legacies use a second pull vault that lets an owner clear the link
  to an ended legacy, and a clone that handles USDT on every payment path
  and refuses a claim fee above 0.5%; the settings that decide what a new
  wallet legacy is (its code, its vault, the renewal key) wait in the
  48-hour upgrade queue. Existing legacies keep the code they were created
  with. It had internal reviews and adversarial tests, and has not had a
  published audit.
- **Every known compiler bug assessed.** On 2026-11-12 every contract of ours on mainnet was rebuilt from source to the bytes on the chain and checked against every known bug of its compiler version. None is found to apply; for one optimizer bug the answer is a strong indication, weaker for the contracts built with solc 0.8.20, not a proof.
- **Continuous checks on the contracts.** Since 2026-10-29 the contracts repository runs its test suite, coverage thresholds for the Shield Vault and the recovery registry, static analysis and a fuzz and invariant suite on every push. The checks inform; they do not block a merge.
- **Upgrades queued by a Safe.** Since 2026-09-24 the governance Safe
  queues upgrades and a separate hardware key is the only one that can
  cancel them; the maintainer key keeps its proposer role until the Safe is
  2-of-3. See [Upgrade Policy](../architecture/upgrade-policy.md).
- **Safe legacies: the Safe manages notifications too.** Since the
  2026-11-07 upgrade a multisig legacy's own Safe may edit its watchers and
  reminder settings, next to the wallet that created it. The app still
  offers these settings to the creator only; doing it from the Safe needs a
  Safe transaction built by hand for now.
- **The second audit (CDSecurity).** CDSecurity reviewed the full suite, including the timelocks and premium contracts added since the first audit, with the report published October 2025 in [`github.com/CDSecurity/audits`](https://github.com/CDSecurity/audits) and mirrored in [`github.com/10102-labs/audits`](https://github.com/10102-labs/audits). The original legacy contracts were audited by RockSolid Security ([report January 2025](https://github.com/10102-io/computing-sc/blob/main/Security_Review_Computing_Will.pdf)). No published audit report covers anything deployed since October 2025.
- **Permit2 one-confirmation creates.** Creating a Transfer legacy or timelock is now a single confirmation: one signed Permit2 batch replaces the per-token `approve` transactions, and tokens stay in the owner's wallet until claim. This also delivered the single-prompt create flow we previously tracked under EIP-5792 batching.
- **EIP-1167 minimal-proxy clones for EOA legacies.** New EOA legacies deploy as ~45-byte clones of one shared implementation per network, cutting creation gas by 80%+. Existing legacies are unaffected.
- **EOA activity auto-renew.** Opt-in Premium feature: an attestor observes the owner's public wallet activity and renews the inactivity timer for them, with every safety bound on-chain. See [EOA Activity & Auto-Renew](../architecture/eoa-activity-auto-renew.md).
- **Timelocked upgrades.** `DefaultProxyAdmin` is now owned by an on-chain upgrade timelock: every implementation change waits in a public 48-hour queue before it can execute. See [Upgrade Policy](../architecture/upgrade-policy.md).
- **Gas-sponsored claims.** A beneficiary with no ETH can claim by signing a free EIP-712 authorization; 10102's relayer submits the transaction and pays the gas. The app switches to this path automatically when the beneficiary's wallet can't cover the fee. See [Gas-Sponsored Intents](../architecture/gas-sponsored-intents.md).

## Under evaluation

Ideas we think are promising but haven't committed to:

- **SafeNet integration.** Safe Foundation's forthcoming decentralized transaction-security layer. We've drafted a technical brief ([available on request](mailto:info@10102.io)) covering how our Safe Guard interacts with SafeNet's Transaction Guard contract. Nothing to build until Safe Foundation answers it.
- **ERC-8211 smart batching.** Runtime parameter injection and pre/post assertions for multi-transaction flows. Most relevant for the beneficiary-side batch claim path (100+ transfers). An external standard; nothing to build until batching of this kind is mainstream in wallets.
- **EAS attestations.** Using the Ethereum Attestation Service to let third parties attest to legacy validity (trusted auditor attestations, beneficiary co-testimony for conflict resolution). Schema design is the open question.
- **More beneficiary-onboarding paths.** The current "generate a new address" flow is good for people new to wallets but limited. Ideas include passkey-based smart accounts and delegate.xyz-style hot/cold separation for the claim address.
- **AA wallet integration (EIP-4337).** When AA wallets standardize a way to expose "last outgoing transaction timestamp" on-chain, we can collapse the EOA/Safe activity-tracking distinction. Not a standard yet; tracking the ecosystem.

## Deferred

Items we've explicitly decided to _not_ do now, with enough context to pick them up later:

- **An external review of legacy v3 and Shield Vault v3.** Both went to mainnet without one, a risk we accepted and state wherever they are described. The review is still wanted. It is not commissioned and nothing is scheduled; the scope document for a reviewer is ready. Trigger: the value held in v3 legacies or in Shield Vault v3 grows, or an audit firm is engaged for anything else. Findings would go into the next contract upgrade, and since a legacy and a vault cannot be upgraded, into a notice to their owners.
- **A second, independent watcher for admin actions.** Today one service of ours watches the upgrade queue and the router's settings and alerts by email. Not every admin action is alerted yet. A watcher outside our own servers, with a phone channel, is evaluated and waits for a decision. Trigger: that decision, the Safe moving to 2-of-3, or more value in v3.
- **A Safe module adding the Shield Vault's delay and veto to an existing Safe.** Not built: the Shield Vault already serves holdings, and a guard with a bug can block a Safe's transactions. Trigger: Safe owners ask for it, or an audit budget is set.
- **What estate planners asked for beyond legacy v3.** Per-share backups and an executor are live. Still open, all off-chain: grouping several legacies and timelocks as one estate, note templates that reference legal documents, and an estate summary or instruction letter to export.
- **Saying how a Shield Vault holding closed.** The status page of a closed holding says only that everything left; the subgraph now has what is needed to name the way and the date. Ready to pick up.
- **Native NFT transfer for Multisig legacies.** NFTs held by the Safe pass with it, because the whole Safe passes. For wallet legacies, NFTs are live with legacy v3 (see Recently shipped): each NFT goes to one named beneficiary rather than being split, and only collections on the list can be named.
- **Save on autopilot outside the US.** Kept to US residents for now, for regulatory reasons. Trigger: demand from elsewhere, after counsel.
- **Wallet passes for gifts** (Apple and Google). Blocked on issuer accounts we do not have.
- **Recurring USDC to ETH conversion.** The obstacle is custody and regulation, not technology: moving a user's funds on a schedule is a different kind of service.
- **A combined portfolio view across watched wallets.** It needs balance and price data across chains that we do not index. Trigger: enough people watching several wallets, or such data adopted for another feature.
- **Multichain deployment beyond mainnet + Sepolia.** Technically straightforward but substantially increases operational surface. We'd rather do mainnet + Sepolia extremely well than ship on 5 chains with half-working cross-chain UX.

A more granular list, with triggers for when each item becomes urgent, lives alongside the code (`docs/DEFERRED.md` in the `computing` repo, `docs/plans/` and `docs/security/` in `computing-sc`), so `git log` is the audit trail.

## How to influence this

- **Security issues** → `security@10102.io`. We take these seriously and will disclose per responsible-disclosure norms.
- **Feature requests and bug reports** → issues on [`github.com/10102-io/computing-sc`](https://github.com/10102-io/computing-sc/issues).
- **General feedback** → `info@10102.io`.

We don't promise to ship everything that's requested, but we read all of it and it genuinely shapes what moves from "under evaluation" to "active work."
