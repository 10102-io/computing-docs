---
description: >-
  How email reminders are driven from on-chain state — an off-chain
  reminder-worker reads PII-free notify signals, keeps recipient emails
  encrypted, and sends them, while the plan keeps working even if email
  delivery fails.
---

# Email Reminders

Email reminders are an opt-in Premium capability that gives owners and beneficiaries out-of-band notifications at key moments in a legacy's lifecycle. They are **additive**: the on-chain contract doesn't depend on email delivery, and the whole email layer can fail without affecting the legality or executability of a legacy.

For the user-facing behavior (what events trigger emails, who gets what), see [Configure Email Reminders](../user-guide/premium-features/configure-email-reminders.md). This page covers the architecture.

{% hint style="info" %}
**Architecture change (2026-06-02).** Reminders used to run on-chain through Chainlink Automation (a daily cron) and Chainlink Functions (the `PremiumMail*` consumers that called the mail service), with recipient emails stored on-chain. That path is **retired** — see [Retired: the Chainlink path](#retired-the-chainlink-path) below. Reminders now run from an off-chain `reminder-worker`, and recipient emails are **no longer stored on-chain**.
{% endhint %}

## Components

| Component | Role |
|---|---|
| Router contracts | Deploy the per-legacy contracts at legacy creation, and emit PII-free notify events (e.g. `LegacyEmailNotifyRequested`) at activation. Not otherwise involved in reminders. |
| `PremiumSetting` | Stores reminder configuration **as addresses only** — which beneficiaries to notify and the advance-notice windows. No emails or names are stored or emitted on-chain. |
| `PremiumReminderView` | A standalone, stateless, read-only contract. The worker polls it (`dueRemindersBatch`) to learn which time-based reminder windows are currently open for a legacy. Falls back to a 7-day window when a creator hasn't set their own. |
| `reminder-worker` | Off-chain service (Railway + Postgres). Holds recipient emails/names **encrypted at rest**, reads notify signals from the subgraph and `PremiumReminderView`, dedupes, and dispatches email. |
| `email-proxy` | The existing 10102 mail service the worker posts to. Fills in template variables and hands off to the SMTP provider. |
| Mailjet | SMTP-as-a-service provider behind `email-proxy` that actually puts email in recipients' inboxes. |
| The Graph subgraph | Indexes the PII-free `LegacyEmailNotifyRequested` event into a `NotifyRequested` entity (subgraph v0.4). The worker reads these to know when an activation/reset happened. |

The on-chain pieces (`PremiumSetting`, `PremiumReminderView`, routers) live in the public [`computing-sc`](https://github.com/10102-io/computing-sc) repository. The `reminder-worker` and `email-proxy` services live alongside the frontend in `computing/services`.

## Where recipient emails live

Recipient emails and names are **off-chain and encrypted**:

- Each recipient is one row keyed by `(chainId, legacy, recipientAddress)`. The `email` and `name` fields are encrypted with **AES-256-GCM** under a **per-record key**, derived (HKDF) from a master key plus a per-row salt.
- The master key (`EMAIL_ENC_KEY`) is a deployment secret held by the hosting platform and is **never** stored in the database.
- A per-record key means a single recipient can be erased independently: deleting the row **crypto-shreds** it (its key material is gone, so the ciphertext is unrecoverable). This is how erasure / GDPR "right to be forgotten" is satisfied.
- The worker is **PII-minimal**: the only personal data it stores is the recipient's own name/email. Everything else in an email (owner address, legacy name/type, beneficiary addresses, amounts) is reconstructed from public on-chain / subgraph data at send time.

## Authenticating to the worker

The frontend talks to the worker with **EIP-712 wallet signatures** — no shared secret ever ships in the browser. The owner signs a typed message and the worker recovers the signer and checks it against the legacy's on-chain `creator()`. To avoid one signature per recipient, the save/erase/read flows are **batch-signed once per legacy**:

| Endpoint | Purpose | Who may sign |
|---|---|---|
| `/ingest-legacy` | Save the legacy's complete recipient set (recipients absent from the list are crypto-shredded) | The legacy's on-chain `creator()` |
| `/recipients` | Load the decrypted recipients back so the owner can re-display / edit their config | The legacy's on-chain `creator()` |
| `/erase-legacy` | Wipe the legacy's entire recipient set | The legacy's on-chain `creator()` |
| `/send-instructions` | Send the manual "send instructions" email to a legacy's recipients | The legacy `owner` (also checked `creator() == owner` on-chain) |

The worker **fails closed** if it can't read `creator()` on-chain. "Send instructions" runs entirely server-side — the browser never sees the recipient emails — and is throttled by a **per-owner cooldown stored in Postgres** (replacing the old client-side guard).

## Workflow

### Step 1 — Subscription and configuration

1. The owner purchases a Premium subscription (paid in ETH, USDC, or USDT to the `PremiumRegistry` contract). The subscription is on-chain, time-bound, and scoped to the paying wallet.
2. While Premium is active, the owner configures reminders. The **addresses** to notify and the advance-notice windows are written **on-chain** to `PremiumSetting`; the matching **emails/names** are synced to the `reminder-worker` over an EIP-712-signed `/ingest-legacy` call.
3. No cron registration happens on-chain. The off-chain worker discovers legacies to watch from the subgraph and the on-chain configuration.

### Step 2 — Evaluation

The worker runs on a schedule (its own tick, not an on-chain cron). Each tick does two passes:

1. **Event pass** — pulls recent `NotifyRequested` entities from the subgraph (owner-reset, multisig activation, transfer activation). For transfer activations it reconstructs per-recipient ERC-20 amounts from the activation transaction's `Transfer` logs.
2. **Due pass** — calls `PremiumReminderView.dueRemindersBatch(...)` for the watched legacies to learn which **time-based** windows are open (e.g. "approaching activation", "ready to activate", second-line windows). The chain is the trust anchor for *when* a reminder is due.

### Step 3 — Reminder dispatch

Both passes funnel into a single dispatch step:

1. Resolve the recipient rows for the legacy.
2. Claim an idempotency key in the **sent-ledger** (`claim-before-send`) so the same `(legacy, recipient, event)` is only ever sent once — even across worker restarts or subgraph re-org replays.
3. Decrypt the recipient's email **in memory only** and POST to `email-proxy`, which fills template variables and delivers via Mailjet.
4. Record the provider message id. A failed send releases the claim so the next tick retries.

### Resolved: no more duplicate emails

The old Chainlink path executed on multiple oracle nodes that each made an independent mail call, and the mail service didn't dedupe — so **the same reminder could arrive more than once**. A single worker with a **claim-before-send sent-ledger** removes that fan-out entirely: each reminder is claimed once and sent once. This previously-documented limitation is **resolved**.

## Retired: the Chainlink path

The following on-chain email machinery has been **decommissioned**:

- **Chainlink Automation** — the per-user upkeep crons (via `PremiumAutomationManager`) that called `checkUpkeep` / `performUpkeep` to evaluate when a reminder was due.
- **Chainlink Functions** — the `PremiumMail*` consumer contracts that bridged the "send email" signal to the off-chain mail call.

Scheduling moved to the worker's own tick + `PremiumReminderView`; delivery moved to the worker + `email-proxy`. On **mainnet**, the teardown completed on **2026-06-02**: the upkeeps were cancelled, the Functions subscription was cancelled, and remaining LINK was withdrawn. `PremiumSetting` was slimmed in the same cutover — emails/names are no longer stored or emitted, activation triggers are emit-only, and the spoofable mail-trigger path was removed.

{% hint style="info" %}
This page covers **email reminders** only. The separate Chainlink/Moralis activity-oracle described in [Indexing & Activity Tracking](indexing-and-activity-tracking.md) and [Inactivity Detection](../dev/technical-analysis.md) is a different subsystem and is documented there.
{% endhint %}

## Privacy and opt-in

- **Email addresses are not on-chain.** They're held off-chain by the worker, encrypted at rest with a per-record key, and only decrypted in memory at send time. A reader of the chain sees only addresses and reminder windows, never emails or names.
- Reminders are strictly opt-in per recipient. An owner cannot secretly enroll a beneficiary's email; the flow requires the owner to input the address explicitly, and the worker only accepts writes signed by the legacy's on-chain creator.
- A recipient can be erased independently (crypto-shred), and deleting the legacy wipes its recipient set.
- 10102 never asks a beneficiary to sign anything to "enable reminders." If a recipient ever gets an email claiming they need to sign or connect a wallet to continue receiving reminders, it's not us.

## What happens when email breaks

If the email layer is entirely broken (worker down, `email-proxy` down, Mailjet down), the only consequence is that reminder emails don't send. The legacy itself continues to work exactly as specified on-chain. A beneficiary whose reminder never arrives can still activate on time via the app or via Etherscan — using the [Legacy Claim Card](../user-guide/legacy/legacy-claim-card.md) — because the on-chain state has all the information needed.

This is the "plan survives us" principle in action: email reminders are a _better experience_, never a _requirement_.
