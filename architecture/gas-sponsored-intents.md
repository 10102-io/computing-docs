---
description: >-
  How beneficiaries claim without holding any ETH — signature-authorized
  intents the contract verifies on-chain, relayed and paid for by 10102.
---

# Gas-Sponsored Intents

The hardest moment in an inheritance product is the claim: the beneficiary is
often new to Ethereum, holding a freshly generated wallet with a balance of
exactly zero — and an on-chain claim costs gas. Gas-sponsored intents close
that gap: **the beneficiary signs a free message, and 10102 submits the
transaction and pays the fee.** The same mechanism lets an owner reset their
inactivity timer without paying gas.

## The one-sentence version

**You sign an EIP-712 authorization (free); any relayer — normally ours — can
submit it, but the contract only ever acts as the signer, for the signer.**

## How it works

The EOA legacy router exposes signature-authorized twins of its two
identity-bound entrypoints:

| Direct call (you pay gas) | Sponsored twin (relayer pays gas) | Who signs |
| --- | --- | --- |
| `activeLegacy` — claim | `activeLegacyFor` | the beneficiary |
| `activeAlive` — owner check-in | `activeAliveFor` | the owner |

Instead of proving identity with `msg.sender`, the sponsored twins recover it
from an EIP-712 signature (domain `10102 Legacy Sponsored`, bound to the
router's address and the current chain). The recovered signer is then used
exactly where `msg.sender` would have been — the relayer's own address plays
no role in what happens.

A claim authorization binds, in one signature:

- **the beneficiary** (recovered signer — funds only ever go to their own
  allocation),
- **the specific legacy** (its id),
- **the exact asset list** being claimed (hashed into the signature — the
  relayer cannot add, drop, or reorder assets),
- **a single-shot nonce** (each signer has a sequential on-chain nonce; a
  signature is consumed exactly once and can never be replayed),
- **a deadline** (expired intents are worthless; check-in signatures
  additionally cannot be dated more than one hour ahead, so a relayer cannot
  hoard one and submit it months later).

Anyone who changes their mind about an outstanding signed intent can cancel
it on-chain by advancing their nonce (`invalidateSponsorNonce`).

## What a malicious relayer could do

Submit your intent, or not submit it. That is the whole list. It cannot
redirect funds (they flow to the recovered signer's own allocation), cannot
alter what is claimed (the asset list is inside the signature), cannot replay
(single-shot nonce), and cannot act without a signature that you produced.
The worst case — a relayer that silently drops your intent — degrades to the
world before this feature existed: you submit the direct call yourself.
Relaying is deliberately **permissionless** for the same reason; if our
relayer disappears, anyone can run one.

## Owner control

Sponsored **claims are on by default** for every EOA transfer legacy. An
owner who does not want third parties submitting claim transactions for
their legacy can turn them off (and back on) at any time with
`setSponsoredClaimsEnabled` — the direct, beneficiary-pays claim path is
always available regardless.

## 10102's relayer

The app wires this in automatically. When a beneficiary starts a claim and
their wallet cannot cover the network fee, the app switches from "send a
transaction" to "sign an authorization", forwards it to our relay service,
and tracks the resulting transaction exactly as if the beneficiary had sent
it. Beneficiaries with ETH simply pay their own fee as before.

The relay service applies operational bounds of its own — none of which are
load-bearing for safety (that all lives on-chain):

- it simulates the call first, so an intent that would revert is rejected
  for free instead of wasting gas;
- daily relay caps (global and per-legacy) and a gas-price ceiling bound the
  worst-case spend;
- **claims are relayed free for everyone.** Sponsored **check-ins are a
  premium perk** — an owner resetting their own timer is alive, able to pay,
  and doing something recurring, so we cover that fee only for subscribers.
  This is a policy choice in the relayer, not a protocol rule: the contract
  entrypoints themselves don't distinguish.

## Honest limits

- **Not gasless — gas-sponsored.** Someone still pays; for claims it's us.
  The daily caps mean that in an extreme rush the relayer can decline and
  beneficiaries fall back to paying their own fee.
- **Smart-contract wallets are supported** (ERC-1271 signature validation,
  including Safe), with one inherent caveat of that standard: a contract
  wallet decides validity at verification time, so rotating its owners can
  invalidate an intent that is already signed but not yet submitted.
- **This is the signed-intent generation, not account abstraction.** The
  long-term end-state for fee abstraction is EIP-7702/paymasters; this
  mechanism is compatible with that future and doesn't block it.

## Related pages

- [Legacy contracts created with EOAs](legacy-contracts-created-with-eoas.md)
- [Create flow v2](create-flow-v2.md)
- [EOA activity auto-renew](eoa-activity-auto-renew.md) — the other
  "10102 pays the gas" mechanism, for automatic timer renewals
- [Upgrade Policy](upgrade-policy.md)
