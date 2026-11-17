---
description: >-
  Name someone you trust who can tell your legacy that you have died, once
  you have been silent for half your check-in period. Your heirs can then
  claim 30 days after the confirmation, if that is sooner. Nobody verifies
  it, and any check-in from you cancels it. Premium only.
---

# Name an Executor

A legacy activates when your check-in period runs out. With a long period, your heirs may wait a year or two after your death. An **executor** shortens that wait: someone you trust, like a lawyer or a friend, who can confirm to your legacy that you have died.

An executor shortens the wait only when your check-in period is longer than 60 days: they can confirm after half the period, and claims open 30 days after that. With a 12-month period the earliest claim date is 7 months after your last check-in.

It is available on wallet legacies of the new version (created since 2026-11-13 20:42 UTC, or [moved](../legacy/move-to-the-new-version.md) to it), and naming one needs Premium.

{% hint style="warning" %}
**Nobody verifies the confirmation.** Neither the contract nor 10102 checks that you have died. The executor's word, once you have been silent for half your check-in period, plus 30 more days of your silence, is all it takes. Name only someone you trust. "Executor" here is a role in the smart contract only: it does not appoint an executor of your estate under any law.
{% endhint %}

## What the executor can and cannot do

When the executor confirms that you have died:

- The claim date moves to 30 days after the confirmation, if that is sooner than it would be.
- Our server looks for confirmations and emails the owner, so a mistaken confirmation can be answered. It emails you only if you created a reminder for this legacy with your own email (see below), and like any email it can be late or fail.
- A check-in by you cancels it, and so does any change that sends a transaction to the legacy (heirs, shares, backups, the check-in period, NFTs, the executor). A change to a name or the note is saved on your device, sends nothing and cancels nothing. Automatic renewal counts only wallet activity from after the confirmation.
- The executor never receives anything and cannot claim.

And the bounds:

- They can confirm **only after you have been silent for half your check-in period**. With a 12-month period, that is 6 months after your last check-in.
- They can confirm once per 30 days.
- An executor cannot be you, one of your heirs, a backup heir, your backup beneficiary or your second backup.
- If your heirs can already claim, a confirmation changes nothing, and the executor's page says so.

## Naming one

On the legacy's page, in the **Executor** section:

1. Type the **Executor wallet address** and press **Name executor**. Confirm in your wallet.
2. The section then shows "Send them their page" with the link to their **Executor page**. Copy it and send it to them.
3. Create a reminder for this legacy in **Settings**, **Email reminders**, with your own email as the owner's, so you hear about a confirmation. A reminder set for another legacy does not count. Without one, no email can reach you.

**Change executor** names someone else; **Remove executor** removes them, and needs no Premium. Naming or removing an executor counts as a check-in.

## The executor's page

```
app.10102.io/executor/1/<legacy address>
```

The page is public and reads the chain directly. It says what confirming does and where things stand now:

- "The owner last checked in on (date). Confirming becomes possible on (date)": half the check-in period after that check-in, if the owner stays silent.
- "You can confirm now."
- "A confirmation from (date) is running. Another is possible from (date)."
- "The owner's heirs can already claim (since (date)), so confirming would change nothing.", or that the legacy has ended.

To confirm, the executor connects the wallet you named, ticks "I know the owner has died. I understand they are emailed and that any check-in by them cancels this.", and presses **Confirm the owner has died**. It is one transaction, paid by the executor. A wallet that is not the executor's is told so and cannot confirm.

## If your executor confirms and you are alive

Check in. Any check-in cancels the confirmation, and so does any change that sends a transaction to the legacy (heirs, shares, backups, the check-in period, NFTs, the executor). A change to a name or the note is saved on your device, sends nothing and cancels nothing.

- If you created a reminder for this legacy with your own email, our server emails you at that address once it sees the confirmation.
- The legacy's page shows it at the top of the Executor section: "Your executor said on (date) that you have died. Your heirs can claim from (date). If you are alive, check in now: any check-in cancels it.", with a **Check in** button. That button sends an ordinary transaction from your wallet; for a free check-in with Premium, use the page's main **Check in**.
- You have until the claim date the page shows: 30 days from the confirmation, or sooner if your check-in period ends first.

If you no longer trust the executor, remove them after checking in.

## Honest limits

- **The email is your alarm, and email can fail.** It goes to the owner's email of this legacy's reminder, through our server and our email provider. If this legacy has no such reminder, or the email is late, lost or not sent, the legacy's page is the only place that shows the confirmation. Look at it from time to time.
- **30 days is the whole margin.** If you cannot check in within 30 days of a mistaken confirmation (illness, travel, a lost wallet), your heirs can claim.
- **The executor's power is narrow, and it is real.** They can never receive or move anything, but once you have been silent for half your period they can bring the claim date forward.
- **Not independently audited.** The new version of the wallet legacy, the executor included, has had internal adversarial reviews and automated testing and analysis, and no independent audit. See [Legacy](../legacy/README.md#honest-limits-of-the-new-version).

## See also

- [Backup Beneficiaries and Backup Heirs](./manage-contingent-beneficiaries.md)
- [Configure Email Reminders](./configure-email-reminders.md): where the reminder address is saved.
- [Automatic Renewal](./automatic-renewal.md)
- [Activate a Legacy Contract and Claim Funds](../legacy/activate-a-legacy-contract-and-claim-funds.md)
