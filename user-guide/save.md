---
description: >-
  Put a little aside, locked until a date you choose. Save with just your
  email (autopilot locks what you send), or lock from your own wallet. Opens
  on the day you pick.
---

# Save (a monthly lock)

Save is the plainest page in the app: one card, three choices, one button.
It is for someone who has never used a wallet and wants to set money aside
that they cannot touch until a date they choose.

Underneath it is a regular [Timelock](timelock/README.md). The page adds
nothing new on the blockchain; it only removes the choices you do not need.
Every lock you make here also shows in the Timelock tab, where you take it
back once it opens.

Open it at `app.10102.io/save`. There are two ways to save:

- **With just your email** (autopilot): no wallet app, no network fees.
  You send USDC to a wallet that opens with your email, and it is locked
  for you. US residents only for now.
- **From your own wallet**: you lock USDC or ETH yourself, one amount at a
  time, and pay the network fee.

## Saving with just your email (autopilot)

1. **Sign in with your email.** You get a six-character code; a passkey
   works too once you have added one. The first time, this creates your
   savings wallet. It is yours: it runs on Turnkey, a separate company that
   keeps the keys in secure hardware. You write its recovery phrase on
   paper before anything else.
2. **Pick your date.** At least 90 days away. Every lock autopilot makes
   for you opens on that exact date.
3. **Turn autopilot on.** You give 10102 one narrow permission: to lock the
   USDC that arrives in this wallet into a timelock in your name, opening
   on your date. It cannot send your money anywhere else, not to us and not
   to anyone. The permission lasts a year at most; you are reminded before
   it ends, and you can stop at any time.
4. **Send USDC whenever you like.** From Coinbase or any other app, on the
   Ethereum network. Once at least 20 USDC has arrived, autopilot locks all
   of it, usually within the hour, at most once a day and up to 10,000 USDC
   in any 30 days. It emails you when it is done. Other tokens are left
   alone.

On your date, sign in and take your savings back, to this wallet or
straight to another. It is free: 10102 pays the network fee.

### Who sent the USDC: checked before every lock

Before each lock, autopilot checks every address that sent USDC to your
email wallet against the Chainalysis sanctions list, which is published on
Ethereum.

- **A transfer of 0 USDC is ignored.** It moves nothing, and anyone can
  send one, so it can never stop your plan.
- **USDC from a listed sender is kept aside.** We never lock it, never send
  it for you and never pay a fee to move it.
- **Under 20 USDC from listed senders**, your plan carries on and locks the
  rest. We email you once.
- **From 20 USDC**, your plan stops. Nothing is locked or moved, and we
  email you. Your existing locks still open on their date, and the wallet
  is still yours, but we no longer pay its network fees.

You did nothing wrong if this happens: anyone can send USDC to any address.
USDC paid out of a 10102 timelock is checked against the people behind that
lock, unless it is your own savings coming back. Questions:
[info@10102.io](mailto:info@10102.io).

### Plans made before 2026-11-09

Plans set up before 2026-11-09 keep working as they are: each of their
locks opens about an hour before your date. Their page shows **Confirm the
exact date**. One signature switches the plan, and every new lock opens on
your date exactly. Locks already made keep their date.

{% hint style="info" %}
**Honest limits.** Autopilot is offered to US residents only for now, and
not yet in California or New York. 10102 pays the network fees for now (it
tops up the little ETH your wallet needs); it may stop doing that, will
tell you first, and autopilot may pause. If the autopilot key were ever
stolen it still could not send your money anywhere: every lock it could
make opens on the date you chose. On a plan made before 2026-11-09 and not
yet switched to the exact date, the worst it could do is lock USDC in your
own name for longer than you chose. This is a step away from pure
self-custody: you depend on your email account and on Turnkey's service to
sign in, and the recovery phrase is your way out without either.
{% endhint %}

## Making a lock from your own wallet

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

### Funding it from Coinbase, once a month

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

When a lock's date arrives it shows **Ready** in the Timelock tab and in
**Your locks** on [Home](home.md). Tap **Withdraw** there, or **Claim
Assets** on the lock's page, confirm once, and the money is back in your
wallet. With Premium, that confirmation is a free signature and 10102 pays
the network fee
([Gas-Free Check-ins & Claims](premium-features/gas-free-check-ins-and-claims.md)).
ETH locked here is held as wrapped ETH (WETH) while locked; you can have it
back as ETH.

With autopilot, take your savings back from the autopilot page: **Take it
back, free**, to your email wallet or straight to another wallet.

## What you should know

- **No early withdrawal.** A lock opens on its date and not a day sooner.
  Nobody can open it early, including us.
- **No interest.** You get back exactly what you put in.
- **Not a bank account.** No bank holds the money and no deposit protection
  covers it. It sits in an audited contract on Ethereum, in your name.
- **Prices move.** ETH goes up and down. USDC is made to stay close to one
  dollar.
- **Fees.** From your own wallet, locking is a transaction and so is taking
  it back; you pay the network fee for each, except that Premium makes
  taking it back free. With autopilot, 10102 pays them for now, taking your
  savings back included.
- **Upgrades wait in public.** The contracts that hold locks can be
  upgraded, and every upgrade waits in public for at least 48 hours first.
