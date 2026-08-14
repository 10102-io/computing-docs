---
description: >-
  The 10102 partner program: share a link, get verifiably attributed on-chain
  purchases, reconcile every statement yourself on Etherscan.
---

# Partner Program

10102 Computing Legacy is free to use; revenue comes from the optional Premium plan ($199/year or $499 lifetime, paid on-chain). Partners who introduce their audience or clients to the app earn a share of the Premium purchases they bring — with an attribution system designed so you never have to take our word for a number.

This page is the practical guide. The legally binding mechanics live in the published [Partner Program Terms](https://10102.io/partner-terms) (EN and FR).

## How it works

1. We agree on your partner code, for example `acme`.
2. You share your link: `https://app.10102.io/?via=acme` — use this exact host form. No signup, no dashboard, no integration work.
3. When someone arrives through your link, their browser remembers your code for **90 days** (last qualifying click wins). If they buy Premium in that window, the purchase is attributed to you.
4. We settle monthly in USDC on Ethereum, with a statement listing the purchases behind the number.

## Why you don't have to trust us

Premium is bought on-chain, so every attributed purchase corresponds to a real, public Ethereum transaction. Our attribution ledger refuses to record anything it cannot verify against the chain:

- The transaction must be mined, successful, and contain a Premium purchase event from the real registry contract.
- Each purchase can be credited **once**, ever — replayed or duplicate reports are rejected.
- The system stores no personal data: an attribution is a transaction hash, a wallet address that is already public on-chain, and your code.

Your statement gives you the transaction references, and you can confirm every one of them on Etherscan yourself.

## Self-serve stats

Every partner receives a private stats token (`pt_…`). It is scoped to your code only and never exposes purchaser addresses:

```bash
curl -H "Authorization: Bearer pt_your_token" \
  https://api.10102.io/referral-stats
```

The response contains your totals — `purchases`, `unique_purchasers`, first and last purchase timestamps, and `active_subscribers` — plus your most recent attributed transactions.

`active_subscribers` is computed live from the chain: it counts your unique purchasers whose Premium is still active right now (lifetime plans stay counted; expired annual plans drop off). This is the number volume bonuses are measured against.

Treat the token like a password. If it leaks, we rotate it on request; rotation is always explicit, so routine account changes never silently invalidate it.

## Commission and payout terms

| Tier | Commission | Best for |
| --- | --- | --- |
| Standard | 20–25% of the first purchase | Creators, communities, media |
| Advisor / B2B | 10–15% recurring on renewals, plus volume bonuses | Advisors and businesses bringing ongoing clients |

Exact rates are set in your partner agreement; we keep deal terms private on both sides.

- Monthly settlement in USDC on Ethereum, to a wallet you control.
- Balances under $100 roll over to the next month. If participation ends, the final balance pays out in full, even below that threshold.
- Statements can be disputed within **60 days** of issue.
- Either party may end participation with **30 days' written notice**; attributions already recorded are honored at the terms they were earned under.

## Discount mode (fee-barred professionals)

Some partners — notably French avocats and notaires — are legally barred from taking a percentage of a client's purchase. For those relationships the code runs in **discount mode**: the buyer gets a reduced Premium price instead, and the partner receives no commission. The stats token works the same way, so you can still see and reconcile your attributed activity. Commission and discount never both apply to the same purchase. If your profession or engagement letter forbids referral fees, say so when we set up your code.

## Attribution through AI agents

If you build on the [MCP server](agents-and-builders.md) with a partner key, your key can carry your referral code: setup links your integration generates then attribute Premium purchases by the clients your agent onboards to your partner account — the same verified ledger, the same statement.

## Fair play

No self-referral, no paid-search bidding on our brand terms, no representing yourself as 10102, and no marketing that promises investment returns. Purchases that are refunded, reversed, granted as promotions, or made by the partner themselves do not qualify. Partners are independent parties responsible for their own taxes and for the disclosure rules that apply to them (for example, telling your audience a link is a referral link where required).

## Getting started

Send us two things: the code you'd like, and the USDC address for payouts — [info@10102.io](mailto:info@10102.io). Your link works the same day.

Where this page and the published [Partner Program Terms](https://10102.io/partner-terms) differ, the published terms control; a signed partner agreement controls over both.
