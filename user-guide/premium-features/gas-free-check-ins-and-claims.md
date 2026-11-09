---
description: >-
  10102 can pay the network fee for you: claims are sponsored for every
  beneficiary, check-ins and timelock withdrawals for Premium owners, and Save
  take-backs for every email wallet. You sign a free message; we send the
  transaction.
---

# Gas-Free Check-ins & Claims

Every action on Ethereum costs a network fee, and the person who needs to act isn't always the person holding ETH. A beneficiary may hold a freshly generated wallet with a balance of exactly zero. An owner checking in pays a fee for what amounts to pressing a button.

Gas sponsoring closes both gaps with the same machinery: **you sign a free authorization message, and 10102's relayer submits the transaction and pays the fee.** The signature is verified by the contract itself, so the relayer physically cannot do anything other than what you signed.

## What is free, and for whom

| Action | Free for | Where |
|---|---|---|
| Claiming a wallet legacy | Every beneficiary whose wallet cannot cover the fee | The legacy's page |
| Checking in on a wallet legacy | Premium owners | The legacy's page, **Check in** |
| Withdrawing your own timelock | Premium owners | The timelock's page, and **Your locks** on [Home](../home.md) |
| Checking in on a Shield Vault holding | Premium owners | The [Shield Vault](../shield.md) page |
| Taking back savings from Save on autopilot | Every email wallet | [Save on autopilot](../save.md), **Your locks** |

Heirloom claims, and stopping or releasing a Shield Vault holding, are free too; their pages explain how.

## Claims: free for every beneficiary

This one isn't a Premium feature; it's part of the product's core promise. When a beneficiary starts a claim and their wallet can't cover the network fee, the app automatically switches from "send a transaction" to "sign a free message". The signature authorizes exactly the claim they were already entitled to (their address, that specific legacy, the exact asset list, usable once), and 10102 submits and pays. Beneficiaries who do hold ETH simply pay their own fee as before.

Owners stay in control: sponsored claims are on by default and can be switched off per legacy on-chain. The beneficiary-pays path always works regardless.

The step-by-step flow is in [Activate a Legacy Contract and Claim Funds](../legacy/activate-a-legacy-contract-and-claim-funds.md#claiming-without-eth).

## Check-ins: free for Premium owners

On a wallet legacy, **Check in** resets your countdown. With Premium, the button asks your wallet for a free signature instead of a transaction. 10102 sends the check-in and pays the network fee. If the free send is not possible right now, your wallet sends a normal check-in and you pay the fee. Either way the check-in is the same.

Claims are sponsored for everyone because a claim is a once-in-a-lifetime moment for someone who may own no ETH at all. Check-ins recur for owners who can pay, so we cover those for Premium subscribers.

Safe legacies don't need this: every Safe transaction already resets their countdown. For a hands-off version, see [Automatic renewal](./automatic-renewal.md): it renews your timer from your normal wallet activity, and 10102 pays every renewal.

## Timelock withdrawals: free for Premium owners

When one of your own timelocks opens, **Withdraw** in **Your locks** on [Home](../home.md), or **Claim Assets** on the timelock's page, asks for a free signature. 10102 sends the withdrawal and pays the fee. If the free send is not possible, your wallet sends a normal withdrawal.

The money always goes to your own wallet. The signature names you as the recipient and no one else, the timelock pays out only to its recipient, and our relay refuses any request that names another destination.

This covers timelocks your wallet created for itself, standard or soft. A gift you sent belongs to its recipient. A timelock held by a Safe is withdrawn by the Safe, on its page.

## Save take-backs from an email wallet

When a lock made by [Save on autopilot](../save.md) opens, **Take it back, free** sends the savings to your email wallet, or straight to another wallet you type. 10102 pays the fee. No Premium needed.

## When a free send is not possible

The relay has limits, so a free send can be declined:

- **The network fee is too high.** Above a ceiling, the relay does not send.
- **Today's limit is reached.** Free Premium transactions share a daily limit across all owners. Claims and take-backs have their own, so a busy day of check-ins never switches off claims.
- **We could not read the network.** Nothing is sent. Try again in a few minutes.

For a check-in or a timelock withdrawal, your wallet then sends a normal transaction instead, where it can, and you pay the fee. The page tells you why, for example: "Free Premium transactions have reached today's limit, so your wallet sends this one instead." A beneficiary's claim falls back to a normal claim from their wallet, which needs a little ETH. A take-back from an email wallet has no paid path in the app: try again later.

## Never sent twice

A free send can be slow to answer. Before the app offers you a paid transaction, it reads the signature's nonce on-chain. If the signature was already used, or its transaction is on its way, the app waits for it and sends nothing else. If our relay answers that a copy of the same request is already running or already sent, the app waits too. You see: "It was sent for you and is still confirming, so nothing else was sent. Check back in a minute."

So a check-in, a withdrawal or a claim never goes out twice, and you never pay for one we already sent.

## Why this is safe

The relayer is a courier, not a signer. The contract recovers *your* signature and acts as you, for you:

- A claim authorization can only send assets to the signer's own allocation, exactly as the owner configured it. Nobody, including 10102, can redirect it.
- A withdrawal authorization pays only the timelock's recipient, which for your own timelock is you.
- Every authorization is single-use (an on-chain nonce) and expires (a deadline you signed).
- If our relayer ever went away, nothing is lost: relaying is permissionless on-chain, and the ordinary pay-your-own-fee path always works.

The full mechanics and trust model are in [Gas-Sponsored Intents](../../architecture/gas-sponsored-intents.md).

## Common questions

**Is "gas-free" really free?** For you, yes. Someone still pays the network, and for sponsored actions that someone is 10102. The relayer applies fair-use limits (daily caps, a gas-price ceiling); if it declines, the app falls back to the normal self-paid path where one exists.

**Can a relayer see my keys or assets?** No. It only ever receives the signed message you produced, and the contract checks that message before anything happens. Your keys never leave your wallet.

**I'm an owner and I don't want any third party submitting claims for my legacy.** You can turn sponsored claims off per legacy by calling `setSponsoredClaimsEnabled` on the router (from Etherscan today; an in-app switch is on the way). Your beneficiaries will then pay their own network fee (gas) to claim.
