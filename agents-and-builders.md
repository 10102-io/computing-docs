---
description: >-
  How AI agents and developers can discover and use 10102 Computing Legacy: the
  Guardian AI guide, ERC-8004 discovery, the MCP server, and pay-per-call
  inference.
---

# Agents & Builders

This page is for two audiences: developers building software (including AI agents) that needs to read or set up on-chain legacies, and autonomous agents that act on behalf of their users. It explains how to discover 10102 Computing Legacy as a registered agent, what you can call for free, and how metered inference works.

The product is non-custodial and fully on-chain. Nothing here takes custody of funds, and no endpoint can move assets on its own: every contract write requires the user's wallet signature.

## Meet Guardian: the AI digital-legacy guide

Guardian is the AI assistant inside 10102 Computing Legacy. It helps people set up their digital legacy and timelock contracts: it interviews a wallet owner about goals, helps them choose a matching contract type (Legacy or Timelock), and builds pre-filled setup links that the user completes by connecting a wallet and signing on-chain.

- **Powered by Venice AI.** Guardian runs on Venice's privacy-preserving inference. Conversations are not stored and are not used to train models. The chat model is GLM 5.2.
- **Free allowance for everyone.** A free inference allowance applies per caller, so anyone can use Guardian without bringing their own credits.
- **A guide, not an advisor.** Guardian helps you configure legacy and timelock contracts. It does not give legal, tax, or financial advice.

## Discover us as an agent (ERC-8004)

10102 is registered under [ERC-8004](https://eips.ethereum.org/EIPS/eip-8004), the agent-discovery standard. Agent identities live in an Identity Registry implemented as an ERC-721 on Ethereum mainnet, so any agent or tool can resolve our identity on-chain.

- **Agent name:** 10102 Guardian
- **Agent id:** `34821`
- **Agent registry:** `eip155:1:0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`

The identity is discoverable through standard ERC-8004 explorers such as [8004scan.io](https://8004scan.io).

### A2A agent card

The agent capabilities are described by an A2A agent card (protocol version 0.3.0):

```
https://app.10102.io/.well-known/agent-card.json
```

The card advertises these skills:

- **Suggest a legacy plan** (`recommend-legacy-plan`): interview a wallet owner about goals and help them choose the matching contract type.
- **Build pre-filled setup URL** (`build-setup-url`): generate a deep link that pre-fills the web form with beneficiaries, durations, name, and type. The user finishes by connecting a wallet and signing on-chain.
- **Query legacy status** (`query-legacy-status`): read LegacyDetail records (status, beneficiaries, triggers, Safe context) from the subgraph or on-chain router views.
- **Query timelock status** (`query-timelock-status`): read Timelock entities (unlock time, lock type, recipient, token contents, withdrawal status).
- **Verify on-chain deployment** (`verify-contract-deployment`): confirm canonical router addresses and read live contract state via Ethereum mainnet RPC.

## Model Context Protocol (MCP)

Guardian exposes a read-only MCP server:

```
https://mcp.10102.io/mcp
```

(MCP protocol version `2025-06-18`.)

Reads are free. The MCP server lets you look up legacies and timelocks and generate pre-filled setup links. There are no keys to manage and no write paths: the server returns indexed state and deep links, but contract creation still requires the user's wallet signature in the app or via a direct contract call.

In practice that means an agent can:

- Query indexed Legacy and Timelock state for an address.
- Build a pre-filled setup URL that hands the user off to the web app to review and sign.
- Read a wallet's portfolio-health summary (below).

### Portfolio health

Agents can read a wallet's token-health summary: the same readout the app's dashboard widget shows, powered by the Computing Tokens research pipeline ([tokens.10102.io](https://tokens.10102.io)). Two ways in:

- **MCP tool:** `get_portfolio_health` returns the cached report for an address: an average health score and any flagged holdings.
- **REST proxy:** `GET https://mcp.10102.io/portfolio-health?address=0x…` returns the same headline numbers as plain JSON. It's CORS-open (browser apps can call it directly) and exists because the upstream research API key stays server-side; it never ships in a client bundle.

The REST proxy is rate-limited per IP, and responses are cached per address for a few minutes. Both surfaces are privacy-shaped: they report on **public on-chain holdings only**, nothing an observer couldn't derive from the chain itself. A wallet that has never been scanned gets a "no report" response; the owner can run a free scan at tokens.10102.io to generate one.

### Quantum readiness

The `get_quantum_readiness` MCP tool reports, for any address: whether it is an EOA or a contract account, whether its public key is already exposed on-chain (any EOA that has signed exposes it permanently, which is the class of funds a future quantum computer could target first), and whether the account has registered a post-quantum recovery commitment in the on-chain [QuantumRecoveryRegistry](https://etherscan.io/address/0xaB3C8C69fD17ba980b3D11064200c866904e360E#code), with the earliest registration timestamp. Everything it reads is public chain state. Registration itself happens in the app dashboard (one transaction); see [Quantum Readiness](architecture/quantum-readiness.md) for the honest threat model behind the readout.

## Partner access (B2B)

The anonymous MCP surface above is free and stays open; partner keys never restrict public access. Partners building products on top of it (estate-law practices, wallet providers, agent platforms) can additionally get a **bearer key** for the same read-only tool surface:

- Send it as `Authorization: Bearer <key>` on your MCP requests; the partner context is bound to your MCP session when it initializes.
- A key does not unlock extra data or any write path; the tools are identical to the public ones. What it adds is identification (so we can support and reason about your integration) and attribution: setup links your integration builds carry your partner reference instead of the generic one.
- A key can also carry your **referral-program code**: setup links then attribute Premium purchases by the clients your agent onboards to your partner account, on the same on-chain-verified ledger as regular referral links. See [Partner Program](partners.md).
- An unknown key is rejected with `401` rather than silently falling back to anonymous access, so a typo is visible immediately.

Partner keys are provisioned manually. To request one, reach out via [GitHub](https://github.com/10102-io/computing) or [info@10102.io](mailto:info@10102.io).

## Pay-per-call inference (x402)

Guardian chat inference is available through an OpenAI-compatible endpoint (the venice-proxy):

```
https://venice-proxy-production.up.railway.app/v1/chat/completions
```

- It is **OpenAI-compatible**, so you can point an existing `/v1/chat/completions` client at it.
- A **free tier** applies per caller, subject to a rate limit.
- For unmetered, metered-by-you usage, bring your own Venice credits via [x402](https://www.x402.org) (the `X-Sign-In-With-X` header). The agent registration advertises `x402Support: true`, so you pay per call from your own wallet credits rather than relying on the shared free allowance.

## Build with us

- **Source code:** the contracts and routers are open source at [github.com/10102-io/computing-sc](https://github.com/10102-io/computing-sc).
- **Agent docs:** the canonical agent onboarding doc is published at [AGENTS.md](https://github.com/10102-io/computing/blob/main/AGENTS.md).
- **Subgraph:** indexed Legacy and Timelock state is served from The Graph gateway: `https://gateway.thegraph.com/api/subgraphs/id/FXzZDbZxzdWaYGTPEcxN2AatbucRooQWfY67moam8oCd`.
- **Non-custodial and on-chain:** every flow is operable directly from the Ethereum contracts. Reads are free, setup is a handoff to a wallet signature, and no part of this stack can move a user's assets without that signature.
