---
description: >-
  The "why" behind the harder engineering decisions in 10102 Computing
  Legacy — useful for auditors, integrators, and future maintainers.
---

# Design & Engineering Notes

This section is for readers who want to understand why the app is shaped the way it is, not just what it does. If you're picking up the codebase — contributing, auditing, or integrating against it — this is the place that explains the trade-offs the [Architecture](../architecture/README.md) section takes as given.

We deliberately kept this section small. There are thousands of small decisions in the codebase; only a handful are load-bearing enough to warrant a page here.

## What's in this section

- [Inactivity Detection](technical-analysis.md) — the hard problem at the heart of the product, and why the EOA and Safe paths look different.
- [Core Mechanisms](core-mechanisms.md) — the system's moving parts (CREATE2, upgradeable proxies, heartbeat, the router pattern) explained at the level of "what each piece is responsible for and why it exists."
- [Roadmap](backlog.md) — public ledger of planned work and explicitly deferred items, with enough context to understand each trigger condition.

## Principles we try to live by

A few opinions we fall back on when we're not sure which way to go:

- **Plan survives us.** Every feature must have a path to activation that doesn't require our UI, servers, or company. This is the single most important principle. It shows up as the Legacy Claim Card, as verified contracts on Etherscan, as public source code and audits, as multisig-controlled upgradeability.
- **On-chain authoritative, off-chain additive.** Anything our backend or any third-party service (e.g. Mailjet for the off-chain reminder worker) contributes must be verifiable against on-chain state, and its absence must degrade UX but not correctness. Activation itself takes no off-chain input — there is no activity oracle in the trust path.
- **Two thin routers beat one fat one.** The EOA and Safe legacy flows share ~80% of their semantics, but the 20% that differs is different enough that conflating them makes both paths carry each other's complexity. The same applies to the three timelock flavors (Timelock / Soft / Gift) being distinct code paths.
- **Explicit is better than clever.** The EOA legacy flow is two separate transactions (deploy, then approve) instead of one wallet-magic bundle, because the explicit version is easier to audit and easier for users to reason about. We're tracking EIP-5792 `wallet_sendCalls` to get the magic _back_ without losing the audibility.
- **Fail closed on activation.** We bias every uncertain case toward _not_ activating early — a delayed real claim is recoverable (the owner heartbeats, the beneficiary retries once the window elapses), a premature disbursement is not. For EOAs this is why the inactivity timer only resets on the owner's own legacy interactions rather than on a best-effort guess about their wider wallet activity.

## What's _not_ here

We deliberately don't publish:

- The specific signer set of the `DefaultProxyAdmin` Safe. Available on request; not broadcast to reduce targeting.
- Operational runbooks, incident playbooks, or internal monitoring dashboards. Those belong in a private ops repo, not in user-facing docs.
- Unsigned/unpublished upgrade plans. When we do upgrade, the plan is published before the transaction.

## Where things actually live

- **Contracts + tests + audits** → [`github.com/10102-io/computing-sc`](https://github.com/10102-io/computing-sc)
- **Audit reports** → [`github.com/10102-labs/audits`](https://github.com/10102-labs/audits)
- **Contract addresses (per network)** → `contract-addresses.json` in the `computing-sc` repo
- **Security disclosures** → `security@10102.io`
- **General questions** → `info@10102.io`
