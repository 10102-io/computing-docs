---
description: >-
  What Home shows once your wallet is connected: Your plan, a short list of
  facts about your plans read from the chain, and Your locks, your timelocks
  with the date each one opens.
---

# Home: your plan and your locks

Home is the first page of the app (`app.10102.io`). Once your wallet is connected, it shows what your 10102 plans cover on this network, what to look at, and what you can take back.

At the top, **Covered** adds up, in dollars, what your legacies, timelocks and Shield Vault holdings cover. Next to it, **Your plan** lists the facts behind it. **Your locks** follows when you have at least one timelock.

## Your plan

Your plan is a short list of facts about your plans on this network. Each line comes from the chain and the indexer, so every device shows the same list. Nothing kept in one browser, such as your watch list, changes it. A legacy's name can differ between devices unless you [sign in on each device](./authentication.md#sign-in-on-each-device).

There is no score, grade or level: each line is a fact you can check, with at most one action.

The summary at the top reads "Nothing to look at right now." or "2 things to look at." Only lines marked **Look at this** count, and each of those needs something done.

| Line | What it tells you | When it says **Look at this** |
|---|---|---|
| **This wallet's plan** | How much of this wallet's value its own legacy would deliver: "$X of $Y in this wallet would reach the people you named." ETH is shown apart. | More than $150 is outside any plan and less than 90% is covered. |
| **Timer** (one per legacy) | The date the legacy can activate unless you check in, and whether automatic renewal can really run before then. | The date is within 30 days or has passed, or automatic renewal is on but cannot run. |
| **Backup person** | Whether someone is named in case your first beneficiary cannot claim. | The account that created the legacy has Premium and nobody is named. |
| **Reminder email** | Whether an email warns you before the legacy can activate. | The account that created the legacy has Premium and no reminder is set up. |
| **Wallet code** | Whether this wallet runs a contract's code (an EIP-7702 delegation). | It runs code from a contract we do not recognise. |

Each line carries a word as well as an icon: **OK**, **Look at this**, **To note** (a fact worth knowing that needs nothing yet), **With Premium** (a Premium feature you don't have), or **Not checked** (a read failed; try again later).

### This wallet's plan

Only this wallet's own legacy can deliver its tokens. ETH cannot be given a token permission, so it is counted apart: a legacy reaches ETH once it is wrapped as WETH, and Stake & Shield is another way. A Safe legacy covers the Safe, not the wallet you connected with. The action is **Add assets**, **Start a wallet legacy** or **Stake & Shield**, whichever fits.

### Timer

For a wallet legacy, the line reads "Your legacy can activate on (date) unless you check in." and then says what automatic renewal really does:

- **On, and able to run**: while this wallet is active, we reset the timer near that date.
- **Off**: shown as **To note** until the date is 30 days away. It says whether renewal is off, comes with Premium, or cannot cover a waiting period longer than about 13 months.
- **On, but unable to run** (**Look at this**): Premium has ended, Premium ends before the last 30 days open, renewals are paused on 10102 right now, the 12 months of renewals after your last check-in end before that date, or the waiting period is longer than about 13 months.

When Home shows the Premium notice (your Premium ends within 14 days, or has ended, while automatic renewal is on), the timer points to that notice instead of saying it twice. See [Automatic Renewal](./premium-features/automatic-renewal.md).

For a Safe legacy, the line reads "unless your Safe makes a transaction": any Safe transaction restarts its timer.

### Backup person and Reminder email

Both are Premium features of the account that created the legacy. For a Safe legacy, that may be another signer. Without Premium, the line says **With Premium**, with **See Premium** when you created the legacy; it is not a failure. A multisig legacy, which hands over the Safe itself, has no backup layers, so the Backup person line leaves it out.

### Wallet code

A wallet that signed an EIP-7702 delegation runs a contract's code, and whoever controls that code can move everything in the wallet. A known wallet's smart account is named and reads **OK**. Code from a contract we do not recognise reads **Look at this**, with a link to [wallet alerts](./wallet-alerts.md). Token approvals you gave other apps are not checked here; the line links to Revoke.cash for that.

{% hint style="warning" %}
**These checks are not a security review of your wallet.** In the app's words: "These checks read your 10102 plans on this network. They are not a security review of your wallet." An empty list means nothing in your 10102 plans needs doing, not that your wallet is safe.
{% endhint %}

## Your locks

Your locks lists the timelocks you hold, standard or soft, from your wallet or a Safe, with the date each one opens. Gifts you sent are not listed: they belong to their recipient, and you cannot take them back.

- **Ready** locks come first, with **Withdraw**. A lock held by a Safe shows **Withdraw with your Safe**, which opens its page so the Safe can sign.
- A lock still counting shows "Opens on (date)."
- A soft lock not yet unlocked shows how long it opens after you start the unlock.
- Each name opens the lock's page.

With Premium, **Withdraw** on your own wallet's lock is a free signature and 10102 pays the network fee. The money goes only to your wallet. See [Gas-Free Check-ins & Claims](./premium-features/gas-free-check-ins-and-claims.md#timelock-withdrawals-free-for-premium-owners). Without Premium, it is a normal transaction.

## The rest of Home

- **Shield Vault** and **Stake & Shield**: protect a holding against a stolen key. See [Shield Vault](./shield.md).
- **Assets elsewhere**: other wallets and Bitcoin addresses you watch, and your [wallet alerts](./wallet-alerts.md).
- **Quantum readiness**: its own card, a dated proof that you owned your wallet before any quantum break. It moves nothing and stops nothing. See [Quantum Readiness](../architecture/quantum-readiness.md).
- **Save** and **Heirloom**: put money aside until a date, or send a gift that opens later. See [Save](./save.md) and [Heirloom](./heirloom.md).

Not sure what a line means? **Ask Guardian** on Home, and pick "Explain my plan on Home".
