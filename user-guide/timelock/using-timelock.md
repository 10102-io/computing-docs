---
description: >-
  Creating, managing, and claiming each of the three timelock flavors:
  Timelock, Soft Timelock, Beneficiary Timelock.
---

# Using Timelock

Navigate to the **Timelock** tab of the app. You'll see any timelocks you've created, and any beneficiary timelocks others have set up with you as the recipient.

To create a new one, click **Create a timelock** and pick a flavor: Timelock, Soft Timelock, or Beneficiary Timelock.

## Common concepts

These apply to all three flavors.

### Approving and including assets

A timelock holds what you explicitly approve for it. For each asset:

- **ERC-20 token**: approve the timelock contract as a spender, then include it. The asset moves into the contract when you finalize creation.
- **ETH**: can't be approved directly. Under **Lock with ETH**, enter an amount and pick WETH under **Receive as token**. The ETH goes with the create transaction and the lock holds it as WETH, 1 WETH for 1 ETH. There is nothing to approve for it.
- **Staked ETH** (Ethereum mainnet): under **Stake ETH and lock it**, enter an amount of ETH. It is staked with Lido and the lock holds the wstETH you get for it. See [Stake ETH and lock it](#stake-eth-and-lock-it) below.

{% hint style="info" %}
**Shortcuts.** **Use the most I can** fills in the ETH your wallet holds, less what it keeps for network fees. **Protect all my tokens** plans the full balance of each token. Pick single tokens and amounts instead when you want precise control over which tokens and how much.
{% endhint %}

{% hint style="warning" %}
**Finalizing fails if your approvals don't cover the configured amounts.** If you set a timelock to lock 100 USDC but only approved 50, creation will revert with an `ERC20: transfer amount exceeds allowance` error. The UI guards against this by not letting you finalize until approvals match, but if you edit approvals in your wallet out-of-band, re-check the UI before finalizing.
{% endhint %}

### Stake ETH and lock it

On Ethereum mainnet the **Choose your assets** step has a card named **Stake ETH and lock it**. It does two things that already exist on the chain, one after the other:

1. **The stake.** Your ETH is sent to Lido's wstETH contract, which stakes it with Lido at Lido's own rate and gives your wallet wstETH for it. No exchange and no market price is involved. The card shows about how much wstETH your amount becomes before you confirm.
2. **The lock.** That wstETH is locked like any other token you hold.

{% hint style="warning" %}
**The lock holds wstETH, not ETH, and wstETH is what comes out.** You withdraw wstETH when the lock opens; in a Beneficiary Timelock the recipient does. The app does not turn it back into ETH. That is a step of its own afterwards: Lido's withdrawal queue, which can take days, or a market. The card and the line under **Finalize creating timelock** say this before you confirm anything.
{% endhint %}

How many confirmations it takes depends on your wallet:

- **A wallet that batches** (it confirms several steps as one): type the amount and finalize. One confirmation stakes the ETH, approves the wstETH and creates the timelock. Either all three happen or none does.
- **Any other wallet**: three confirmations. **Stake ETH** on the card, then **Approve wstETH** on the card, then **Finalize creating timelock**. While an amount is typed and not staked yet, Finalize stays off, so a lock is never created beside ETH you meant to stake.

**If you stop after the stake**, the wstETH is in your wallet and nothing is lost. When you come back to **Choose your assets**, the card reads the wallet from the chain, says how much wstETH it holds, and offers **Lock my wstETH**. From there it is the lock step only. wstETH you already held before works the same way.

Honest limits:

- **wstETH is Lido's staking token.** Its amount stays fixed while its value in ETH follows Lido's staking rewards. Those vary, Lido takes its fee from them, and nobody promises them. Staking is provided by Lido, not by 10102, and the wstETH carries the risks of Lido's protocol for as long as you hold it, in the lock and after.
- **Lido's rate changes once a day.** In a batching wallet the lock asks for exactly the wstETH the stake gives at the rate read just before you confirm. If the rate changes in between, nothing is staked or locked and you finalize again.
- **Ethereum mainnet only.** Lido is not on the Sepolia test network, so the card is not shown there. The smallest stake the card takes is 0.01 ETH, and it keeps enough ETH back for network fees.
- **On Create timelock only.** A legacy's page does not stake. For staked ETH in a wallet legacy, see **Keep it in my wallet** under [Stake & Shield](../shield.md).

### Status values

| Status | Meaning |
|---|---|
| **Active** | Locked. Funds can't be moved. Progress bar shows how much of the lock has elapsed. |
| **Unlock pending** (Soft Timelock only) | You've initiated an unlock; the waiting period is counting down. Still can't claim yet. |
| **Ready** | Lock period has expired. You (or the recipient, for a Beneficiary Timelock) can claim. |
| **Claimed** | Funds have been claimed back. Timelock is done. |

## Timelock

### Create

1. **Timelock** tab → **Create a timelock** → select **Timelock**.
2. Configure:
   - **Name**: private label.
   - **Duration**: how long to lock for. Supports any duration up to contract limits.
   - **Assets to include**: pick which tokens and amounts. Use **Lock with ETH** for ETH, **Stake ETH and lock it** for ETH you want staked with Lido, and approve/include individual tokens.
3. Finalize. One transaction per flavor:
   - ETH you added is sent with it and wrapped as WETH in the same transaction.
   - The timelock is created and assets move in.

### Claim

Once the lock elapses:

1. Open the timelock's details page; the status will read **Ready**.
2. If the lock was made with **Lock with ETH**, choose **Claim as ETH** (preselected) or **Claim as WETH**. A lock made with **Stake ETH and lock it** has no such choice: it pays wstETH.
3. Click **Claim assets**. Sign one transaction.
4. Assets return to your wallet.

Your open timelocks are also listed in **Your locks** on [Home](../home.md), which opens from the timelocks chip, with a **Withdraw** button.

Timelocks carry no delivery fee: the 0.25% that applies when heirs claim a legacy does not apply when you take a timelock back, or when a recipient claims one. You pay network gas.

{% hint style="info" %}
**Free with Premium.** For a timelock your wallet created for itself, standard or soft, Premium turns that transaction into a free signature and 10102 pays the network fee. The assets go only to your wallet. If the free send is not possible right now, your wallet sends it and you pay the gas. See [Gas-Free Check-ins & Claims](../premium-features/gas-free-check-ins-and-claims.md#timelock-withdrawals-free-for-premium-owners).
{% endhint %}

## Soft Timelock

### Create

Same as Timelock, except the duration field is replaced by a **waiting period**: "once unlock is initiated, how many days until funds can be claimed?"

### Unlock

From the timelock dashboard or the details page, click **Unlock**. Sign one transaction. The status changes to **Unlock pending** and the waiting period begins.

While unlock is pending, the timelock is still protected; funds can't be claimed. This is the window during which you'd notice a compromise and take defensive action (cancelling the unlock, moving other funds, rotating keys).

{% hint style="info" %}
**Cancelling an unlock.** From the details page, you can cancel an in-progress unlock and return to the locked state. Useful if you initiated the unlock yourself and changed your mind, or if you suspect the unlock was initiated by an attacker with access to your keys. In the attacker case, though, they could also just cancel your cancellation, so rotate keys in parallel.
{% endhint %}

### Claim

Once the waiting period elapses, status reads **Ready**. Click claim, sign, done.

## Beneficiary Timelock

### Create

Same flow as Timelock, with two extra fields:

- **Recipient address**: the wallet that will be able to claim when the lock expires. Must be an EOA.
- **Name**: something meaningful to the recipient, since they'll see it in their dashboard.

{% hint style="info" %}
**The recipient is notified by the presence of the timelock on their dashboard.** They'll see it in the Timelock tab as the designated recipient, starting as soon as you finalize creation. If you want it to be a surprise, consider sharing the information at delivery time rather than at creation time. For a designed gift experience with a claim link, printable sheets, and free claiming, use [Heirloom](../heirloom.md) instead.
{% endhint %}

### Claim (by the recipient)

1. The recipient opens the Timelock tab; the timelock appears with them as the recipient.
2. Until the lock elapses, status is **Active**. Once elapsed, status reads **Ready**.
3. Recipient clicks claim, signs the transaction, and receives the assets directly into their wallet.

The creator can't reverse a beneficiary timelock after it's created: once locked, the recipient is the only one who can claim at expiry. If the creator wants flexibility to revoke, a regular Timelock (claimable by them, then sent off later) is the right pattern instead.

## Troubleshooting

- **"Transfer amount exceeds allowance" on finalize.** Your approvals don't cover the amounts you've configured for the timelock. Re-approve from inside the app; the UI keeps approvals and the finalize button in sync.
- **"Cannot claim: still locked".** The contract checks on-chain time, not your browser clock. Double-check the lock-end date on the details page.
- **The ETH amount is refused.** Wrapping ETH as WETH involves no market, so it does not depend on liquidity. The form refuses an amount that leaves too little ETH for network fees and says how much you can lock; **Use the most I can** fills that in.
- **ETH and WETH in one lock.** One lock cannot take ETH and, as a separate token, WETH you already hold: both would be WETH inside the lock. Remove WETH from the plan or clear the amount under **Lock with ETH**, or make two locks.
- **Finalize is off after I typed an amount to stake.** Your wallet does not batch, so the stake is a confirmation of its own: use **Stake ETH** on the card, or clear the amount.
- **"Your wallet could not take this as one confirmation."** Nothing was staked or locked. The card now has **Stake ETH**: stake, approve the wstETH, then finalize.
- **I staked and then closed the page.** The wstETH is in your wallet. Start the timelock again and use **Lock my wstETH** on the card.

## See also

- [Timelock: overview of the three flavors](./README.md)
- [Concepts: Storage token](../concepts.md#storage-token)
