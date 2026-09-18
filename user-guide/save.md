---
description: >-
  Put a little aside every month, locked until a date you choose. Buy on
  Coinbase, send it to your wallet, lock it. Opens on the day you pick.
---

# Save (a monthly lock)

Save is the plainest page in the app: one card, three choices, one button.
It is for someone who has never used a wallet and wants to set money aside
that they cannot touch until a date they choose.

Underneath it is a regular [Timelock](timelock/README.md). The page adds
nothing new on the blockchain; it only removes the choices you do not need.
Every lock you make here also shows in the Timelock tab, where you take it
back once it opens.

Open it at `app.10102.io/save`.

## Making a lock

1. **What are you locking.** USDC if you want the value to stay steady, or
   ETH. The page shows what your wallet holds and starts on whichever you
   have.
2. **How much.** Type an amount, or tap **Use all of it**. For ETH, "use
   all" keeps a little back for the network fee.
3. **When does it open.** In 1, 3, 5 or 10 years. The exact opening date
   is shown before you confirm, and it is final: it cannot be moved earlier,
   not even by you.
4. **Lock it.** One confirmation in your wallet. USDC asks for a second one
   the first time, to allow the exact amount. You pay the network fee,
   usually a few dollars.

The lock appears under **Your locks so far** on the same page, with its
opening date, and in the Timelock tab.

## Funding it from Coinbase, once a month

The page walks through this with your own wallet address ready to copy and
a code to scan:

1. **Let Coinbase buy for you.** In the Coinbase app: Buy, then Recurring
   buy, pick USDC or ETH, and choose weekly or monthly. Your bank money
   turns into USDC or ETH on its own.
2. **Once a month, send it to your wallet.** In Coinbase: Send, paste your
   wallet address, choose the **Ethereum** network (not Base, not any
   other), and send. Try a small amount the first time.
3. **Come back to the Save page and lock it.**

Why monthly and not weekly: every lock is a transaction on Ethereum with
its own network fee. Weekly buys on Coinbase are fine; lock once a month.

Each month's lock opens on its own date. After a few years of monthly locks
you have money opening every month, for as long as you kept saving.

## Taking it back

When a lock's date arrives it shows **Ready** in the Timelock tab. Tap it,
confirm once, and the money is back in your wallet. ETH locked here is held
as wrapped ETH (WETH) while locked; you can have it back as ETH.

## What you should know

- **No early withdrawal.** A lock opens on its date and not a day sooner.
  Nobody can open it early, including us.
- **No interest.** You get back exactly what you put in.
- **Not a bank account.** No bank holds the money and no deposit protection
  covers it. It sits in an audited contract on Ethereum, in your name.
- **Prices move.** ETH goes up and down. USDC is made to stay close to one
  dollar.
- **Fees.** Locking is a transaction, and so is taking it back. You pay the
  network fee for each.
