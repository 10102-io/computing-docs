---
description: >-
  10102 can pay the network fee for you: claims are sponsored for every
  beneficiary, and check-ins are sponsored for Premium owners. You sign a free
  message; we submit the transaction.
---

# Gas-Free Check-ins & Claims

Every on-chain action normally costs a network fee, and the person who needs to act isn't always the person holding ETH. A beneficiary claiming an inheritance may hold a freshly generated wallet with a balance of exactly zero. An owner checking in pays a fee for what amounts to pressing a button.

Gas sponsoring closes both gaps with the same machinery: **you sign a free authorization message, and 10102's relayer submits the transaction and pays the fee.** The signature is verified by the contract itself, so the relayer physically cannot do anything other than what you signed.

## Claims: free for every beneficiary

This one isn't a Premium feature; it's part of the product's core promise. When a beneficiary starts a claim and their wallet can't cover the network fee, the app automatically switches from "send a transaction" to "sign a free message". The signature authorizes exactly the claim they were already entitled to (their address, that specific legacy, the exact asset list, usable once), and 10102 submits and pays. Beneficiaries who do hold ETH simply pay their own fee as before.

Owners stay in control: sponsored claims are on by default and can be switched off per legacy on-chain. The beneficiary-pays path always works regardless.

The step-by-step flow is in [Activate a Legacy Contract and Claim Funds](../legacy/activate-a-legacy-contract-and-claim-funds.md#claiming-without-eth).

## Check-ins: the Premium half

The same mechanism covers the owner's side: resetting your inactivity timer by signature, with 10102 paying the fee. Claims are sponsored for everyone because a claim is a once-in-a-lifetime moment for someone who may own no ETH at all; check-ins recur for owners who can pay, so we cover those for Premium subscribers.

Two ways your check-ins can be gas-free on Premium:

- **[Automatic renewal](./automatic-renewal.md)**: the hands-off version. You don't sign anything at all; the attestor renews for you when your wallet has been active, and 10102 pays every renewal.
- **Sponsored check-ins**: the on-demand version. You sign a free message instead of paying for the check-in transaction. The relayer capability is live; the one-click app experience is rolling out.

## Why this is safe

The relayer is a courier, not a signer. The contract recovers *your* signature and acts as you, for you:

- A claim authorization can only send assets to the signer's own allocation, exactly as the owner configured it. Nobody, including 10102, can redirect it.
- Every authorization is single-use (an on-chain nonce) and expires (a deadline you signed).
- If our relayer ever went away, nothing is lost: relaying is permissionless on-chain, and the ordinary pay-your-own-fee path always works.

The full mechanics and trust model are in [Gas-Sponsored Intents](../../architecture/gas-sponsored-intents.md).

## Common questions

**Is "gas-free" really free?** For you, yes. Someone still pays the network, and for sponsored actions that someone is 10102. The relayer applies fair-use bounds (daily caps, a gas-price ceiling); if it ever declines, the app falls back to the normal self-paid path.

**Can a relayer see my keys or assets?** No. It only ever receives the signed message you produced, and the contract checks that message before anything happens. Your keys never leave your wallet.

**I'm an owner and I don't want any third party submitting claims for my legacy.** You can turn sponsored claims off per legacy by calling `setSponsoredClaimsEnabled` on the router (from Etherscan today; an in-app switch is on the way). Your beneficiaries will then pay their own claim fee.
