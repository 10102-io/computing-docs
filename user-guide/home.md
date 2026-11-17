---
description: >-
  What Home shows once your wallet is connected: the value in your legacy,
  Your setup, a short list of facts read from the chain, the next step, and
  your locks, your timelocks with the date each one opens.
---

# Home: your setup and your locks

Home is the first page of the app (`app.10102.io`). Once your wallet is connected, it shows what is in your legacy on this network, what to look at, and what you can take back.

At the top, **In your legacy** adds up, in dollars, the value in your live legacy contracts, your timelocks and your Shield Vault holdings. Its chips name each part (legacies, timelocks, Shield Vault), and the timelocks chip opens [your locks](#your-locks). Next to it, **Your setup** lists the facts behind it, and under both, **Next step** names the one thing to do first, or says "You're all set for now."

Without a wallet connected, Home opens on "Your assets, your rules." with a **Start** button.

## Your setup

Your setup is a short list of facts about your 10102 setup on this network. Each one comes from the chain and the indexer, so every device shows the same list. Nothing kept in one browser changes it. A legacy's name can differ between devices unless you [sign in on each device](./authentication.md#sign-in-on-each-device).

There is no score, grade or level: each item is a fact you can check, with at most one action.

The card shows a one-line summary, "Nothing to look at right now." or "2 things to look at.", and one word per item. **See details** opens each item's reason and its action. Only items marked **Look at this** count, and each of those needs something done.

| Item | What it tells you | When it says **Look at this** |
|---|---|---|
| **This wallet's setup** | The wallet's total value and how much of it would reach the people you named: "$X of $Y in this wallet would reach the people you named.", then the main reason the rest is not in your legacy yet. | More than $150 is not in your legacy and less than 90% is, or the wallet holds $10 or more and none of it is. |
| **Timer** (one per legacy) | The date the legacy can activate unless you check in, and whether automatic renewal can really run before then. | The date is within 30 days or has passed, or automatic renewal is on but cannot run. |
| **Backup person** | Whether someone is named in case your first beneficiary cannot claim. | The account that created the legacy has Premium and nobody is named. |
| **Reminder email** | Whether an email warns you before the legacy can activate. | The account that created the legacy has Premium and reminders are not switched on. |
| **Wallet code** | Whether this wallet runs a contract's code (an EIP-7702 delegation). | It runs code from a contract we do not recognise. |

Each item carries a word as well as an icon: **OK**, **Look at this**, **To note** (a fact worth knowing that needs nothing yet), **With Premium** (a Premium feature you don't have), or **Not checked** (a read failed; try again later).

### This wallet's setup

On the card, this item's word says what is true: **In your legacy**, **Partly in your legacy**, **Not in your legacy yet**, or **This wallet** where no amount can be stated.

Only this wallet's own legacy can deliver its tokens. ETH cannot be given a token permission, so it is most often the reason: ETH can go into your legacy once it is wrapped as WETH, and Stake & Shield is another way. A few dollars of dust is never the headline. A token without a USD price is said to be left out of the amounts, not counted as zero. A Safe's legacy contract includes only what the Safe holds, not the wallet you connected with. The action is **Add assets**, **Start a wallet legacy** or **Stake & Shield**, whichever fits.

### Timer

For a wallet legacy, the item reads "Your legacy can activate on (date) unless you check in." and then says what automatic renewal really does:

- **On, and able to run**: while this wallet is active, we reset the timer near that date.
- **Off**: shown as **To note** until the date is 30 days away. It says whether renewal is off, comes with Premium, or cannot run for a waiting period longer than about 13 months.
- **On, but unable to run** (**Look at this**): Premium has ended, Premium ends before the last 30 days open, renewals are paused on 10102 right now, the 12 months of renewals after your last check-in end before that date, or the waiting period is longer than about 13 months.

When Home shows the Premium notice (your Premium ends within 14 days, or has ended, while automatic renewal is on), the timer points to that notice instead of saying it twice. See [Automatic Renewal](./premium-features/automatic-renewal.md).

For a Safe legacy, the item reads "unless your Safe makes a transaction": any Safe transaction restarts its timer.

### Backup person and Reminder email

Both are Premium features of the account that created the legacy. For a Safe legacy, that may be another signer. Without Premium, the item says **With Premium**, with **See Premium** when you created the legacy; it is not a failure. A multisig legacy, which hands over the Safe itself, has no backup layers, so the Backup person item leaves it out.

Backup person reads the legacy's backup beneficiary. Backups you gave single heirs on a wallet legacy of the new version are set and shown on the legacy's own page; see [Backup Beneficiaries and Backup Heirs](./premium-features/manage-contingent-beneficiaries.md).

### Wallet code

A wallet that signed an EIP-7702 delegation runs a contract's code, and whoever controls that code can move everything in the wallet. A known wallet's smart account is named and reads **OK**. Code from a contract we do not recognise reads **Look at this**, with a link to [wallet alerts](./wallet-alerts.md). Token approvals you gave other apps are not checked here; the item links to Revoke.cash for that.

{% hint style="warning" %}
**These checks are not a security review of your wallet.** In the app's words: "These checks read your 10102 setup on this network. They are not a security review of your wallet." An empty list means nothing in your 10102 setup needs doing, not that your wallet is safe.
{% endhint %}

## Next step

One sentence and one button, read from the same checks as Your setup, so the two never disagree. Free steps come first (a first legacy, what this wallet holds that is not in your legacy yet, a timer that is close, an unknown wallet code, quantum readiness), then the Premium ones (a backup person, a reminder email), then the wallets you hold elsewhere.

## Your locks

The timelocks chip in the value card says how many of your locks are ready ("2 ready") and opens the list of the timelocks you hold, standard or soft, from your wallet or a Safe, with the date each one opens. Gifts you sent are not listed: they belong to their recipient, and you cannot take them back.

- **Ready** locks come first, with **Withdraw**. When exactly one lock is ready, a **Withdraw** chip that names it sits beside the timelocks chip. A lock held by a Safe shows **Withdraw with your Safe**, which opens its page so the Safe can sign.
- A lock still counting shows "Opens on (date)."
- A soft lock not yet unlocked shows how long it opens after you start the unlock.
- Each name opens the lock's page, and **All timelocks** opens the Timelock page.

With Premium, **Withdraw** on your own wallet's lock is a free signature and 10102 pays the network fee. The money goes only to your wallet. See [Gas-Free Check-ins & Claims](./premium-features/gas-free-check-ins-and-claims.md#timelock-withdrawals-free-for-premium-owners). Without Premium, it is a normal transaction.

## Notices at the top

Home shows a notice above everything else only when something waits for you:

- **A move to finish.** If you started [moving a legacy to the new version](./legacy/move-to-the-new-version.md) and it stopped halfway, **Finish the move** or **Finish adding your tokens** takes you back to it.
- **Token permissions still open.** If a wallet legacy you deleted left token permissions open (a request you declined, a closed tab, or the skip box), a note says so and how many wallet requests clearing takes, with **Clear permissions** and **Dismiss**. The same note shows on the Legacy page. See [Edit or Delete a Legacy Contract](./legacy/edit-or-delete-a-legacy-contract.md#permissions-left-open-after-a-delete).
- **Premium ends soon, or has ended**, while automatic renewal is on. See [Automatic Renewal](./premium-features/automatic-renewal.md#emails-and-the-home-notice).

## The rest of Home

- **Shield Vault** and **Stake & Shield**: protect a holding against a stolen key. See [Shield Vault](./shield.md).
- **Assets elsewhere**: other wallets and Bitcoin addresses you watch, and your [wallet alerts](./wallet-alerts.md). The list follows your wallet to every device you [sign in](./authentication.md#sign-in-on-each-device) on.
- **Quantum readiness**: a dated proof that you owned your wallet before any quantum break. It moves nothing and stops nothing. It opens from the next step or from the details of Your setup. See [Quantum Readiness](../architecture/quantum-readiness.md).
- **Save** and **Heirloom**: put money aside until a date, or send a gift that opens later. See [Save](./save.md) and [Heirloom](./heirloom.md).

On the test network, the products built on legacies and timelocks are not on Home; see [Networks](./authentication.md#networks). In [Cypherpunk Mode](./hide-amounts-and-cypherpunk-mode.md#cypherpunk-mode) there is no Home page like this one: the app opens on your legacies.

The eye button in the header hides every amount on Home, for when you share your screen; see [Hide amounts](./hide-amounts-and-cypherpunk-mode.md#hide-amounts).

Not sure what an item means? **Ask Guardian** on Home, and pick "Explain my setup on Home".
