---
description: >-
  How Safe-owned legacies (Multisig and Transfer) integrate with an existing
  Safe via Safe Guards and Safe Modules, and how the lifecycle events flow.
---

# Legacy Contracts Created with Safe SDK

A [Safe](https://app.safe.global) is a multi-signature smart account on Ethereum: a predefined threshold of owner keys is required to execute any transaction. 10102 integrates with Safe using the two extension points Safe exposes to third-party code: **Guards** and **Modules**.

- [Safe Guards](https://docs.safe.global/advanced/smart-account-guards) — hooks that run on every transaction a Safe executes. 10102 uses a Guard to capture the timestamp of the Safe's last outgoing transaction.
- [Safe Modules](https://docs.safe.global/advanced/smart-account-modules) — code that can execute transactions on behalf of the Safe without the usual multi-sig approval flow. 10102 uses a Module to execute the final activation action (transferring assets, or adding beneficiaries as owners) when the inactivity window elapses.

Two legacy flavors live on this path:

- **Multisig legacy** — hands over control of the Safe itself by adding beneficiaries as co-signers on activation.
- **Transfer legacy (Safe owner)** — transfers specific assets out of the Safe to beneficiaries on activation, proportional to configured allocations.

## Contract roles

| Contract | Purpose |
|---|---|
| `MultisigLegacyRouter` | Creates and updates Multisig legacies. Enforces that edits come from the Safe itself (at threshold). |
| `TransferLegacyRouter` | Creates and updates Safe-owner Transfer legacies. |
| Per-legacy `MultisigLegacyContract` | Stores beneficiaries, activation trigger, name/note for a single Multisig legacy. |
| Per-legacy `TransferLegacyContract` | Stores beneficiaries, allocations, asset list, activation trigger for a single Safe-owner Transfer legacy. |
| `SafeGuard` | Installed on the owner's Safe. Persists `lastTimestampTxs` on every Safe execution. |
| `SafeLegacyModule` | Installed on the owner's Safe. Empowered to execute the activation on behalf of the Safe at threshold-bypass. |

All of these live in the public [`computing-sc`](https://github.com/10102-io/computing-sc) repository; addresses are in `contract-addresses.json`.

## Creating a legacy from a Safe

### Step 1 — Have a Safe

The user needs an existing Safe wallet. If they don't have one, they create it at [app.safe.global](https://app.safe.global) with their chosen signer set and threshold. 10102 does _not_ deploy Safes on your behalf.

### Step 2 — Connect and configure

1. The user connects to the 10102 app as a Safe (via WalletConnect or the Safe app embed).
2. They configure the legacy in the UI: beneficiaries, allocations (Transfer) or threshold-on-activation (Multisig), activation trigger (inactivity window), name/note.
3. The app builds a Safe transaction bundle containing:
   - `setGuard(SafeGuard)` — installs the 10102 guard on the Safe.
   - `enableModule(SafeLegacyModule)` — enables the 10102 module on the Safe.
   - `createLegacy(...)` — calls the appropriate router with legacy configuration.
4. The bundle goes through the Safe's normal multi-sig approval and execution flow: co-signers sign until threshold is reached, then it executes atomically.

### Step 3 — On-chain effects

Once the bundle executes:

- The Safe emits `ChangeGuard` and `EnableModule` events. The subgraph indexes them and marks the Safe as 10102-enabled.
- The Router emits a `LegacyCreated` event (exact name varies by router; see the ABIs). The subgraph creates a legacy entity with the Safe address as creator, the beneficiaries, allocations / threshold, and activation trigger.
- `SafeGuard` initializes `lastTimestampTxs` to the creation block timestamp.

## Activity tracking (the happy path)

Every time the Safe executes _any_ outgoing transaction — not just 10102 ones — the Safe's execution hooks call `SafeGuard.checkTransaction(...)`, which updates `lastTimestampTxs`. This is the entire "heartbeat" mechanism for Safe-owned legacies: no explicit check-in button needed, because normal Safe usage _is_ the check-in. The UI also exposes an explicit `I'm still alive` action for owners who want a deliberate heartbeat.

Because the Guard lives _inside_ the Safe, activity detection for Safe-owned legacies is fully on-chain and doesn't depend on Chainlink/Moralis oracles — unlike pure-EOA legacies. See [Indexing & Activity Tracking](indexing-and-activity-tracking.md) for the EOA story.

## Editing a legacy

Any Safe owner can initiate an edit (beneficiary changes, allocation changes, name/note, activation trigger). The edit is a normal Safe transaction and requires threshold signatures just like the creation. When it executes:

- The per-legacy contract's state is updated.
- The Router emits an `LegacyUpdated` event (name varies); the subgraph updates the entity.
- `SafeGuard.lastTimestampTxs` is updated by the underlying Safe execution — edits implicitly reset the inactivity timer.

One asymmetry worth naming: **off-chain notification settings (watchers, email reminders) are managed by `PremiumSetting`, which gates those edits on the original creator EOA, not the Safe at threshold.** So any Safe owner can edit beneficiaries and activation triggers, but only the single EOA who submitted the original creation transaction can edit watchers or reminder configuration. This is tracked as a known asymmetry to resolve via a future `PremiumSetting` upgrade.

## Deleting a legacy

Delete is also a Safe transaction at threshold. The bundle includes:

- `deleteLegacy(...)` — tells the Router to tear down the per-legacy contract state.
- `setGuard(0x0)` — removes the 10102 guard from the Safe.
- `disableModule(SafeLegacyModule)` — removes the 10102 module from the Safe.

The Safe emits `ChangeGuard`, `DisableModule`, and the Router emits a `LegacyDeleted` event. The subgraph marks the legacy as deleted. The Safe is left in exactly the state it was in before creation: no residual permissions, no residual guards.

Users can also tear down manually via Safe's Transaction Builder at [app.safe.global](https://app.safe.global) — calling `setGuard(0x0)` + `disableModule(...)` on the Safe directly — which is the fallback path if the 10102 UI is ever unavailable.

## Activation — the endgame

When a beneficiary attempts to activate a legacy (via the app, via Safe, or via Etherscan using the [Legacy Claim Card](../user-guide/legacy/legacy-claim-card.md)), the Router checks:

1. The caller is one of the configured beneficiaries (primary, or contingent after their window elapses).
2. `block.timestamp - SafeGuard.lastTimestampTxs >= configuredInactivityWindow`.

If both checks pass, the Router invokes the `SafeLegacyModule` to execute the activation action on behalf of the Safe — bypassing the normal threshold, because the Module is pre-authorized for this one specific action and nothing else.

### Multisig legacy activation

The Module executes `addOwnerWithThreshold(beneficiary_i, newThreshold)` once per beneficiary, then `changeThreshold(configuredThreshold)` to the value the owner specified at creation. The Safe emits `AddedOwner` and `ChangedThreshold` events; the subgraph updates the Safe's owner set and records the legacy as activated. Post-activation, the Safe is now co-owned by the beneficiaries at the new threshold, and everything it holds, stakes, or governs is under their collective control.

### Transfer legacy (Safe owner) activation

The Module executes a series of `transfer` calls — ETH and ERC-20 — moving the configured allocations out of the Safe to the beneficiary addresses. The per-legacy contract emits an `Activated` event; the subgraph marks it activated. The Safe itself is untouched (it just becomes lighter by the allocated amounts).

## Why this design

Keeping the Module strictly single-purpose — authorized only for the activation action, never for arbitrary transactions — preserves the security model of the Safe. No matter how badly the Module were compromised, it can only do the one thing the owner explicitly configured at creation. This is the same pattern Safe itself recommends for third-party integrations.
