---
description: >-
  One page on 10102 Computing Legacy: what it is, what is live, how it is
  built, how it earns, and how to reach us.
---

# About

**10102 Computing Legacy** is a set of audited Ethereum smart contracts, with a thin web app on top, that lets people decide how their on-chain assets pass on and lock funds until a date they choose. Assets stay in the owner's own wallet until the rules they set are met. There is no custody, and every feature works directly from the contracts if the app or the company disappears.

Development started in June 2024. Live on Ethereum mainnet since October 2024, under the `10102-io` GitHub organization since January 2026.

## Status

| | |
| --- | --- |
| Network | Ethereum mainnet (production) and Sepolia (public testnet, same flows) |
| Custody | None. Funds stay in the user's wallet or Safe until activation or unlock |
| Audits | RockSolid Security (January 2025), CDSecurity (October 2025). [Reports](https://github.com/10102-labs/audits) |
| Code | Contracts are open source and verified on Etherscan. [Source](https://github.com/10102-io/computing-sc), [deployed addresses](https://github.com/10102-io/computing-sc/blob/main/contract-addresses.json) |
| Upgrades | Every contract upgrade waits in a public 48-hour on-chain queue. [Upgrade Policy](architecture/upgrade-policy.md) |
| Fees | Core flows are free; users pay network gas only |

## What is live

- **Legacy**: assets split across named addresses, or a Safe handed to co-signers, when the owner has been inactive for a window they set. Optional backup beneficiaries, read-only watchers, email reminders and automatic renewal.
- **Timelock**: ETH or tokens locked until a date, for the owner or for someone else. Includes a soft lock with a short unlock delay.
- **Heirloom**: a sealed gift the recipient opens on a chosen date, delivered by a link and a printable paper key. Claiming costs the recipient nothing; the network fee is covered.
- **Guardian**: an in-app assistant, with private inference through Venice, that explains the product and sets up flows on request.
- **For agents and builders**: a public MCP server and free read endpoints. See [Agents & Builders](agents-and-builders.md).

## How it earns

Premium, paid on-chain in ETH, USDC or USDT: $99 per year or $399 once. And a small fee when assets are delivered (0.25% today), fixed into each contract when it is created and shown before you confirm; never to set one up or keep it. Partners who introduce clients or an audience earn a share of the Premium purchases they bring, reconciled against public transactions. See the [Partner Program](partners.md).

## How it is built

Ethereum for settlement, [Safe](https://safe.global) for all multisig flows, [The Graph](https://thegraph.com) for indexing, [Venice](https://venice.ai) for Guardian's inference, Mailjet for reminder emails. Reminders and emails run off-chain and are optional; activation never depends on them.

## Roadmap and limits

What we are working on, evaluating and deferring is public: [Roadmap](dev/backlog.md). The design notes state the limits out loud, for example how inactivity is detected and what the worst case looks like: [Design & Engineering Notes](dev/README.md).

## Links and contact

- App: [app.10102.io](https://app.10102.io). Add `?network=testnet` to try it on Sepolia.
- Docs: [docs.10102.io](https://docs.10102.io). Site: [10102.io](https://10102.io).
- Partnerships and general questions: [info@10102.io](mailto:info@10102.io)
- Security disclosures: [security@10102.io](mailto:security@10102.io)
