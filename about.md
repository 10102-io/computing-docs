---
description: >-
  One page on 10102 Computing Legacy: what it is, what is live, how it is
  built, how it earns, and how to reach us.
---

# About

**10102 Computing Legacy** is a set of open-source Ethereum smart contracts, with a web app on top, that lets people decide how their on-chain assets pass on and lock funds until a date they choose. Assets stay in the owner's own wallet until the rules they set are met. There is no custody, and every legacy, timelock, gift and Shield Vault holding can be operated directly from the contracts if the app or the company disappears; what runs on our servers (email reminders and alerts, free relays, Save on autopilot, Guardian) would stop. The core contracts were audited twice (reports of January 2025 and October 2025); no published audit report covers what was deployed since, and the table below says exactly which is which.

Development started in June 2024. The first contract went to Ethereum mainnet on [15 August 2024](https://etherscan.io/address/0x57F9B7538F3614ecC5074B72371B38e8f9EF13Fa), the first legacy was created through it on [19 August 2024](https://etherscan.io/tx/0x5ddd2c447983b96274599838306aa03a876b82891f507ad89a3f771f3181efb5), and the code the first audit reviewed followed on 25 and 27 October 2024 ([the 2024 contracts, each with the commit its verified source equals](https://github.com/10102-io/computing-sc#project-history--audits)). Live on mainnet since October 2024, under the `10102-io` GitHub organization since January 2026.

## Status

| | |
| --- | --- |
| Network | Ethereum mainnet (production). Sepolia (public testnet) for legacies and timelocks only: the Shield Vault, Stake & Shield, Save, Heirloom and the email wallet run on mainnet only. [Networks](user-guide/authentication.md#networks) |
| Custody | None. Funds stay in the user's wallet or Safe until activation or unlock |
| Audits | Audited twice (reports of January 2025 and October 2025): RockSolid Security (legacy contracts) and CDSecurity (the full suite, including timelocks and Premium). [Reports](https://github.com/10102-labs/audits) |
| Since the audits | No published audit report covers anything deployed since October 2025. That includes every clone implementation wallet legacies have used (clones began in May 2026) and the one-transaction create of 2026-07-27 (Permit2 permissions, signed gas-free claims, check-ins and timelock withdrawals, consent in the transaction, the create pauses). Internal adversarial review is on record for: `LegacyPullVault`, automatic renewal, `QuantumRecoveryRegistry`, `UpgradeTimelock`, `ShieldVault`, the destination wallet on timelock claims (2026-09-24), the upgrade of 2026-11-07 (`LegacyPullVaultV2`, the clone new legacies used until 2026-11-13, the router settings that now wait in the timelock, and the coupon codes and referrer shares in `PremiumRegistry`), the router change of 2026-11-18 (only the claim that ends a legacy releases the owner's create flag), and the Safe legacy change of 2026-11-21 (a guard whose clock only the Safe moves, and a router that activates a legacy only while it is connected to its Safe). The current wallet legacy (v3, since 2026-11-13) and Shield Vault v3 (since 2026-11-11) went live before an external review, which is still to be commissioned. [Review status](architecture/README.md#review-status) |
| Code | Contracts are open source and verified on Etherscan. [Source](https://github.com/10102-io/computing-sc), [deployed addresses](https://github.com/10102-io/computing-sc/blob/main/contract-addresses.json) |
| Upgrades | Every contract upgrade waits in a public 48-hour on-chain queue. Legacy contracts and the Shield Vault cannot be upgraded at all. Claims, check-ins, deletes and withdrawals can never be paused. [Upgrade Policy](architecture/upgrade-policy.md) |
| Fees | Setting up, checking in and changing a legacy cost network gas only. When the people named in a legacy claim tokens or ETH, a 0.25% fee is taken from what they receive, fixed into each legacy when it is created (never above 0.5% on new legacies; none on legacies created before 2026-09-29; none on NFTs, timelocks or gifts). The Shield Vault takes its fee (0.25% today, fixed when a holding opens) only when a holding is released to the people named in it after the owner's silence, never when the owner withdraws (a holding opened before 2026-11-11 keeps the terms it opened with). Premium is optional |
| Terms | Version `v2026-12`, in force since 2026-11-16. The text is at [10102.io/disclosures](https://10102.io/disclosures), and its hash is on-chain so anyone can check it: [how](architecture/create-flow-v2.md#pii-free-events-consent-in-the-transaction) |

## What is live

- **Legacy**: assets split across named addresses, or a Safe handed to co-signers, when the owner has been inactive for a window they set. Optional contingent beneficiaries, read-only watchers, email reminders and automatic renewal. Wallet legacies created since 2026-11-13 can also name NFTs from a short list of collections (three on mainnet since 2026-11-16) and, with Premium, give each beneficiary up to two backups for their own share and name an executor who can bring the claim date forward after a long silence. An older wallet legacy keeps working as it is, and the app offers a guided move to the new version.
- **Timelock**: ETH or tokens locked until a date, for the owner or for someone else. Includes a soft lock with a short unlock delay.
- **Shield Vault**: a token held behind a wait the owner chooses, with a printed recovery sheet that stops a withdrawal the owner did not start, and an optional release to named people after a long silence. **Stake & Shield** stakes ETH on the way in.
- **Save**: a lock until a date for USDC or ETH, from a wallet, or on autopilot from an email wallet (US residents for now).
- **Heirloom**: a sealed gift the recipient opens on a chosen date, delivered by a link and a printable paper key. Claiming costs the recipient nothing; 10102 pays the network fee.
- **Wallet alerts**: an email when tokens leave a wallet in a transaction it did not send, or when it gives a new permission.
- **Guardian**: an in-app assistant, with private inference through Venice, that explains the product and sets up flows on request.
- **For agents and builders**: a public MCP server and free read endpoints. See [Agents & Builders](agents-and-builders.md).

## How it earns

Premium, paid on-chain in ETH, USDC or USDT: $99 per year or $399 once. And a small fee when assets are delivered (0.25% today), fixed into each contract when it is created and shown before you confirm; never to set one up or keep it. Partners who introduce clients or an audience earn a share of the Premium purchases they bring, paid on chain inside the purchase or settled monthly, and reconciled against public transactions. See the [Partner Program](partners.md).

## How it is built

Ethereum for settlement, [Safe](https://safe.global) for all multisig flows, [The Graph](https://thegraph.com) for indexing, [Venice](https://venice.ai) for Guardian's inference, Mailjet for reminder emails. Reminders and emails run off-chain and are optional; activation never depends on them.

## Roadmap and limits

What we are working on, evaluating and deferring is public: [Roadmap](dev/backlog.md). The design notes state the limits out loud, for example how inactivity is detected and what the worst case looks like: [Design & Engineering Notes](dev/README.md).

## Links and contact

- App: [app.10102.io](https://app.10102.io). Add `?network=testnet` to try legacies and timelocks on Sepolia.
- Docs: [docs.10102.io](https://docs.10102.io). Site: [10102.io](https://10102.io).
- Partnerships and general questions: [info@10102.io](mailto:info@10102.io)
- Security disclosures: [security@10102.io](mailto:security@10102.io)
