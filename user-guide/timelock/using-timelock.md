---
description: >-
  Creating, managing, and claiming each of the three timelock flavors:
  Timelock, Soft Timelock, Timelocked Gift.
---

# Using Timelock

Navigate to the **Timelock** tab of the app. You'll see any timelocks you've created, and any timelocked gifts others have set up with you as the recipient.

To create a new one, click **Create timelock** and pick a flavor: Timelock, Soft Timelock, or Timelocked Gift.

## Common concepts

These apply to all three flavors.

### Approving and including assets

A timelock holds what you explicitly approve for it. For each asset:

- **ERC-20 token**: approve the timelock contract as a spender, then include it. The asset moves into the contract when you finalize creation.
- **ETH**: can't be approved directly. Use the swap action to convert ETH into a supported storage token (WETH, a liquid staking token), then approve and include that token.

{% hint style="info" %}
**Quick setup.** Most timelock create screens offer a one-click "Lock all available ETH" (or similar) shortcut that picks a reasonable default storage token, swaps at market rate, and prepares everything for finalization. Use it when you want the default behavior; switch to manual asset-by-asset selection when you want precise control over which tokens and how much.
{% endhint %}

{% hint style="warning" %}
**Finalizing fails if your approvals don't cover the configured amounts.** If you set a timelock to lock 100 USDC but only approved 50, creation will revert with an `ERC20: transfer amount exceeds allowance` error. The UI guards against this by not letting you finalize until approvals match — but if you edit approvals in your wallet out-of-band, re-check the UI before finalizing.
{% endhint %}

### Status values

| Status | Meaning |
|---|---|
| **Active** | Locked. Funds can't be moved. Progress bar shows how much of the lock has elapsed. |
| **Unlock pending** (Soft Timelock only) | You've initiated an unlock; the waiting period is counting down. Still can't claim yet. |
| **Ready** | Lock period has expired. You (or the recipient, for a Timelocked Gift) can claim. |
| **Claimed** | Funds have been claimed back. Timelock is done. |

## Timelock

### Create

1. **Timelock** tab → **Create timelock** → select **Timelock**.
2. Configure:
   - **Name** — private label.
   - **Duration** — how long to lock for. Supports any duration up to contract limits.
   - **Assets to include** — pick which tokens and amounts. Use quick setup for ETH, or approve/include individual tokens.
3. Finalize. One transaction per flavor:
   - If any ETH needs swapping, the swap happens first.
   - The timelock is created and assets move in.

### Claim

Once the lock elapses:

1. Open the timelock's details page; the status will read **Ready**.
2. Click the claim action. Sign one transaction.
3. Assets return to your wallet.

## Soft Timelock

### Create

Same as Timelock, except the duration field is replaced by a **waiting period**: "once unlock is initiated, how many days until funds can be claimed?"

### Unlock

From the timelock dashboard or the details page, click **Unlock**. Sign one transaction. The status changes to **Unlock pending** and the waiting period begins.

While unlock is pending, the timelock is still protected — funds can't be claimed. This is the window during which you'd notice a compromise and take defensive action (cancelling the unlock, moving other funds, rotating keys).

{% hint style="info" %}
**Cancelling an unlock.** From the details page, you can cancel an in-progress unlock and return to the locked state. Useful if you initiated the unlock yourself and changed your mind, or if you suspect the unlock was initiated by an attacker with access to your keys — though in the attacker case, they could also just cancel your cancellation, so rotate keys in parallel.
{% endhint %}

### Claim

Once the waiting period elapses, status reads **Ready**. Click claim, sign, done.

## Timelocked Gift

### Create

Same flow as Timelock, with two extra fields:

- **Recipient address** — the wallet that will be able to claim when the lock expires. Must be an EOA.
- **Name** — something meaningful to the recipient, since they'll see it in their dashboard.

{% hint style="info" %}
**The recipient is notified by the presence of the gift on their dashboard.** They'll see it under a "Gifts to me" section of the Timelock tab, starting as soon as you finalize creation. If you want the gift to be a surprise, consider sharing the information at delivery time rather than at creation time.
{% endhint %}

### Claim (by the recipient)

1. The recipient opens the Timelock tab; the gift appears under **Gifts to me**.
2. Until the lock elapses, status is **Active**. Once elapsed, status reads **Ready**.
3. Recipient clicks claim, signs the transaction, and receives the assets directly into their wallet.

The creator can't reverse a timelocked gift after it's created — once locked, the recipient is the only one who can claim at expiry. If the creator wants flexibility to revoke, a regular Timelock (claimable by them, then sent off later) is the right pattern instead.

## Troubleshooting

- **"Transfer amount exceeds allowance" on finalize.** Your approvals don't cover the amounts you've configured for the timelock. Re-approve from inside the app — the UI keeps approvals and the finalize button in sync.
- **"Cannot claim: still locked".** The contract checks on-chain time, not your browser clock. Double-check the lock-end date on the details page.
- **ETH didn't swap.** A swap requires routing liquidity. For very small amounts or thin-liquidity storage tokens, the swap may fail. Try a supported storage token with known liquidity (e.g. WETH) or reduce the amount.

## See also

- [Timelock — overview of the three flavors](./README.md)
- [Concepts — Storage token](../concepts.md#storage-token)
