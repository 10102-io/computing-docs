---
description: >-
  Optional email notifications for the owner and beneficiaries at key events
  in a legacy's lifecycle. Premium only.
---

# Configure Email Reminders

Email reminders let the owner and beneficiaries get out-of-band notifications at key moments in a legacy's lifecycle, so nobody has to remember to check the app.

Reminders are **additive**: the on-chain contract doesn't know about emails and doesn't depend on them. If a reminder doesn't reach someone, the plan still works — but reminders make it much harder for something to be silently missed.

## Events that trigger emails

| Event | Who gets it |
|---|---|
| **Upcoming activation** (X days before the trigger window elapses) | Owner + impacted beneficiaries for that stage |
| **Activation timeline reset** (owner made an outgoing transaction or used the heartbeat action) | Owner + impacted beneficiaries for the impending stage |
| **Activation happens** (a stage becomes eligible, or a beneficiary actually claims) | Owner + impacted beneficiaries |
| **Successful claim** (assets are distributed) | Owner + impacted beneficiaries, with a distribution summary |

"Impacted beneficiaries" depends on which stage we're talking about:

- For **primary** activation events: primaries only.
- For **second-line** events: primaries _and_ the second-line contingent (primaries are notified because second-line eligibility affects their claim window).
- For **third-line** events: primaries, second-line, and third-line.

The intent is: everyone who can meaningfully act on the information gets it; everyone else doesn't.

## Configuring

Go to **Settings → Email Reminders → Create a Reminder** and pick the legacy to configure.

You'll be asked for:

- **Email addresses**: one per beneficiary (optional, per-beneficiary) and one for the owner (optional).
- **Advance notice period** (in days): how long before each activation event the "upcoming" reminder should go out. Common values are 7, 14, or 30 days.

You can edit or delete a reminder setup at any time from the same screen.

{% hint style="info" %}
**Emails are opt-in.** A beneficiary who doesn't want to share their email can be left unconfigured; they just won't receive reminders. The plan still functions fully.
{% endhint %}

{% hint style="warning" %}
**For Safe-backed legacies, only the creator can edit reminders.** The creator is the specific Safe signer who submitted the original creation transaction. See [Concepts — Creator vs. signer](../concepts.md#creator-vs-signer-safe-legacies). If you're another Safe signer you can view the reminder configuration but not change it — you'll see a "Notification settings managed by 0x…abcd" banner pointing to the creator.
{% endhint %}

## What emails look like

Emails are informational only — they never contain links asking for signatures or private keys. The app URL in an email is there for convenience, not to act as an authentication factor. Beneficiaries can always go straight to `app.10102.io` (or use the printed [Legacy Claim Card](../legacy/legacy-claim-card.md)) instead of clicking through.

Each email includes the Legacy ID so the recipient can look the legacy up on-chain directly if they prefer.

## Privacy considerations

- **Email addresses are not on-chain.** They're stored off-chain, encrypted at rest, and only used for sending reminders.
- **The owner sees their beneficiaries' emails** in the reminder configuration. If a beneficiary prefers total privacy, they can ask to be omitted.
- **Reminders are tied to the legacy**, not the wallet — deleting the legacy deletes the reminder configuration.

## See also

- [Manage Authorized Watchers](./manage-authorized-watchers.md)
- [Manage Contingent Beneficiaries](./manage-contingent-beneficiaries.md)
- [Concepts — Creator vs. signer (Safe legacies)](../concepts.md#creator-vs-signer-safe-legacies)
