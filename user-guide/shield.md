---
description: >-
  Put a token behind a wait you choose. If your key is ever stolen, anything
  the thief starts waits in plain sight, and your recovery sheet can stop it.
---

# Shield Vault (protect a holding)

A protected holding is a token you put in the Shield Vault, behind a wait. Anything that leaves
it, your own withdrawal included, is public from the moment it starts and
only completes after the wait. If someone steals your wallet key, they can
start a withdrawal but not finish it before you notice, and a recovery
sheet you printed at the start stops it and moves everything to a wallet
you chose in advance. If you go silent for a long time, the holding can go
to the people you name.

Open it from **Shield Vault** in the menu, or at `app.10102.io/shield`. It
runs on Ethereum, in the contract
`0x83074f8519F54AF05f7C48911E432e0C44dBEE69`.

## Making one

Five decisions, one per step. Nothing is sent until the last one.

1. **What to protect.** wstETH, USDC, USDT or WETH, and how much. If you
   hold plain ETH, pick WETH: the page offers to wrap the ETH you need
   first (1 WETH is always 1 ETH, and it turns back into ETH any time).
2. **How slowly it can leave.** 7, 30 or 90 days; 30 suits most people.
   This is how long you have to stop a withdrawal you did not ask for.
   Every withdrawal waits this long, yours included, and the page shows
   the date a withdrawal started today would complete.
3. **Who receives it if you go silent.** Optional. Choose 6, 12, 24 or 36
   months without activity, then up to 10 wallet addresses with a share
   each; the shares must add up to 100%. Anything you do on the holding
   counts as activity, and a check-in is one small transaction.
4. **A recovery sheet.** Recommended. Enter a recovery wallet, ideally a
   fresh address from a hardware wallet that has never signed anything,
   and never the wallet you are connected with. The page makes the sheet
   on your device: a secret, the recovery wallet, a QR code and plain
   instructions. Print it before going on. If you download it to print
   elsewhere, delete the file once it is on paper.
5. **Check and protect it.** The page restates everything in sentences,
   including the first date the holding could go to your people. Then up
   to three confirmations in your wallet: register the sheet, allow the
   vault to take the amount (only if needed; USDT asks twice when an old
   allowance is set), and open the holding. If one is interrupted, come
   back and it continues where it stopped.

## Straight from ETH: Stake & Shield

If you hold ETH and want it earning while it is protected, open **Stake &
Shield** from Home and keep **Maximum protection** selected. Enter an
amount: it is staked with Lido into wstETH in one transaction (or, if you
already hold stETH or wstETH, you can use that instead). The page then
continues straight into the steps above with sensible choices already
made: a 30-day wait and your people after 12 months, both changeable
before you confirm. wstETH keeps the same amount while its value in ETH
grows with staking rewards, and your holding shows what it is worth in
ETH today. Prefer instant access? **Keep it in my wallet** leaves the
staked ETH in your wallet with a legacy backup instead, with no
protection against theft.

## Using it

Your holdings show on the same page, with one main action each:

- **Check in** restarts the silence clock.
- **Add more** puts more of the same token in, under the same settings.
  It counts as a check-in.
- **Withdraw** starts the wait for an amount or for everything, to an
  address you choose. You can cancel it any time before it completes.
- **Change settings** (the wait, the people, the sheet) also waits the
  current delay, so a thief cannot shorten the wait or swap your people
  first.
- **Finish** appears when the wait is over. After that moment anyone can
  finish it, which is why a stop must come before the wait ends.

Each holding has a public status page, `app.10102.io/shield/1/<number>`,
that family can open without a wallet to see what is waiting.

**Email alerts, free and optional.** Where they are available, the page
offers to email you the moment a withdrawal or a change starts on any
holding of your wallet, and weeks before a silence period runs out. It
takes one signature and no transaction; the address is stored encrypted,
used for nothing else, and you can delete it from the same place. Nothing
requires it, and Cypherpunk Mode never asks.

## Stopping a withdrawal with the sheet

If a withdrawal or a change you did not ask for appears:

1. Open `app.10102.io/shield` on any phone or computer and choose **Stop
   it**, or use the button on the holding's status page.
2. Scan the QR code on the sheet with the page's camera button (it works
   on iPhone and Android alike, and the code is read on the device), or
   type the secret and the recovery wallet exactly as printed.
3. The page checks the sheet on your device first. A wrong sheet is
   refused there, costs nothing and never leaves the device.
4. Send it from any wallet with a little ETH for the fee, not only yours.
   Everything in the holding moves to the recovery wallet on the sheet,
   right away, and the holding closes. There is no undo.

A copied sheet cannot rob you: it can only send the holding to your own
recovery wallet. It still ends the holding early, so keep it as safe as a
key. The sheet works against a future quantum computer too, because it is
a secret whose hash was registered in advance, not a signature from your
key.

## If you made a quantum readiness sheet on Home

That older sheet and a Shield Vault recovery sheet are different things.
The readiness sheet registered a dated proof that you owned your wallet
before any quantum break; it moves nothing and stops nothing, and it
cannot be used here. Keep it: the date is what it is worth. To protect
coins now, open a holding and make its recovery sheet, which is tied to
that holding, its recovery wallet and this contract. One sheet per
holding.

## When you go silent

After the silence period passes with no activity, anyone can release the
holding to the people you named, in their shares. If one share cannot be
delivered (for example an address a token has blocked), it is kept for
that address and can be claimed later from the status page; the others
are paid anyway.

## What you should know

- **It stays yours.** Only the wallet that opens a holding can withdraw
  from it. We cannot move it, pause withdrawals or change your settings.
  The governance Safe that owns the contract can only choose which tokens
  it accepts and pause new deposits.
- **Three ways out, no others.** Your own withdrawal after the wait, your
  people after the silence period, or your recovery sheet to your recovery
  wallet.
- **The wait protects you only if you notice.** Turn on the email alerts
  if you like, check the page from time to time, and share the status
  page with family so they can watch too.
- **The vault pays no interest.** You get back exactly the tokens you put
  in. A token that grows by itself, like wstETH, keeps growing while it
  waits.
- **Reviewed, not yet audited.** The contract had an adversarial review
  before deployment and has not had an independent audit yet. It cannot
  be upgraded: a fix would be a new vault, and every holding can leave
  through its own wait.
- **Fees.** Each step is a transaction on Ethereum, and you pay its fee.

How it works underneath, with the threat model:
[Quantum Readiness](../architecture/quantum-readiness.md).
