---
description: >-
  How a Multisig legacy integrates with an existing Safe via a Safe Guard and
  a Safe Module, and how the lifecycle events flow.
---

# Legacy Contracts Created with Safe SDK

A [Safe](https://app.safe.global) is a multi-signature smart account on Ethereum: a predefined threshold of owner keys is required to execute any transaction. 10102 integrates with Safe using the two extension points Safe exposes to third-party code: **Guards** and **Modules**.

- [Safe Guards](https://docs.safe.global/advanced/smart-account-guards): hooks that run on every transaction a Safe executes. 10102 uses a Guard to capture the timestamp of the Safe's last outgoing transaction.
- [Safe Modules](https://docs.safe.global/advanced/smart-account-modules): code that can execute transactions on behalf of the Safe without the usual multi-sig approval flow. 10102 uses a Module to execute the final activation action (adding beneficiaries as owners) when the inactivity window elapses.

One legacy flavor lives on this path today:

- **Multisig legacy**: hands over control of the Safe itself by adding beneficiaries as co-signers on activation. Everything the Safe holds, NFTs included, passes with it.

{% hint style="info" %}
**Retired: the Safe-backed Transfer legacy.** A second flavor, which transferred specific assets out of the Safe on activation, was hidden from the app on 2026-05-01 and retired on 2026-05-18. It never had a legacy on mainnet. Its router's bytecode remains on-chain; the app and the contracts repository no longer reference it. The sections below that mention it are kept for the record. To split specific assets between addresses, use a wallet legacy: [Legacy Contracts Created with EOAs](legacy-contracts-created-with-eoas.md).
{% endhint %}

## Contract roles

| Contract | Purpose |
|---|---|
| `MultisigLegacyRouter` | Creates and updates Multisig legacies. Enforces that edits come from the Safe itself (at threshold). |
| Per-legacy `MultisigLegacyContract` | Stores beneficiaries, activation trigger, name/note for a single Multisig legacy. It is also the **Safe Module**: enabled on the owner's Safe, it is what adds the beneficiaries as owners at activation. The only Safe transactions its code can make are adding owners and setting the threshold. |
| `SafeGuard` | Deployed with the legacy and installed on the owner's Safe. Persists `lastTimestampTxs` on every Safe execution. Two versions exist; see [Guard versions](#guard-versions). |
| `TransferLegacyRouter`, `TransferLegacyContract` | Retired (see above). |

The live ones are in the public [`computing-sc`](https://github.com/10102-io/computing-sc) repository; addresses are in `contract-addresses.json`. The legacy v3 features (NFT items, backup beneficiaries, an executor) exist for wallet legacies only, not for Multisig legacies.

## Creating a legacy from a Safe

### Step 1: Have a Safe

The user needs an existing Safe wallet. If they don't have one, they create it at [app.safe.global](https://app.safe.global) with their chosen signer set and threshold. 10102 does _not_ deploy Safes on your behalf. A Safe can have one legacy; a wallet that signs for several Safes can give each its own.

### Step 2: Connect and configure

1. The user connects a wallet that is one of the Safe's owners and names the Safe.
2. They configure the legacy in the UI: beneficiaries, the threshold after activation, activation trigger (inactivity window), name/note.
3. That owner sends one transaction from their own wallet: `MultisigLegacyRouter.createLegacy(safe, ...)`. The router checks that the sender is an owner of the Safe and that the Safe has no guard yet, then deploys the per-legacy contract and its guard.
4. The app then proposes a Safe transaction with three calls:
   - `setGuard(SafeGuard)`: installs the 10102 guard on the Safe.
   - `enableModule(legacy contract)`: enables the per-legacy contract as a module on the Safe.
   - the Safe calls the guard's `checkTransaction` hook itself. A Safe reads its guard before it executes a transaction, so the guard does not see the transaction that installs it. With this call the guard counts the set-up, and the inactivity window runs from the set-up instead of from the creation.

   On a Safe 1.5.0, with a version 2 guard, there is a fourth call, `setModuleGuard(SafeGuard)`, so that what a module does for the Safe counts as activity too. A module that acts on its own, such as a standing allowance, then restarts the waiting period each time it acts, like a transaction of the owners. The app leaves the fourth call out when the Safe already runs a module guard of its own.

   The transaction goes through the Safe's normal multi-sig approval and execution flow: co-signers sign until threshold is reached, then it executes. Until it does, the legacy contract exists and can do nothing on the Safe.

### Step 3: On-chain effects

- The Router emits `MultisigLegacyCreated`. The subgraph creates a legacy entity with the Safe, the creating owner, the beneficiaries, the threshold and the activation trigger.
- When the Safe transaction executes, the Safe emits `ChangedGuard` and `EnabledModule`. The subgraph indexes them and marks the legacy live once it has seen both.
- `SafeGuard` initializes `lastTimestampTxs` to the creation block timestamp.

## Activity tracking (the happy path)

Every time the Safe executes _any_ outgoing transaction (not just 10102 ones), the Safe's execution hooks call `SafeGuard.checkTransaction(...)`, which updates `lastTimestampTxs`. This is the entire "heartbeat" mechanism for Safe-owned legacies: no explicit check-in button needed, because normal Safe usage _is_ the check-in. The UI also exposes an explicit `I'm still alive` action for owners who want a deliberate heartbeat.

Because the Guard lives _inside_ the Safe, activity detection for Safe-owned legacies is fully on-chain and passive: every Safe transaction counts, with no oracle and no check-in chore. Pure-EOA legacies use a per-legacy timer instead; see [Indexing & Activity Tracking](indexing-and-activity-tracking.md) for that story.

### Guard versions

A guard is deployed once per legacy and is not upgradeable, so a legacy keeps the guard it was created with.

- **Version 1** (every legacy created so far on Ethereum). Its hook is not reserved to the Safe: a call from another address can start the inactivity window again. That can delay an activation; it cannot bring one forward, and it moves nothing.
- **Version 2** (`GUARD_VERSION()` is 2). The time moves on two occasions only: the Safe itself calls a hook, or anyone calls `sync()` while the Safe's transaction count differs from the one the guard noted, which proves the Safe executed a transaction the guard did not see. The guard keeps that count (`lastNonce()`), and none of its hooks can revert, so it cannot stand between a Safe and its transactions.

Version 2 comes with an upgrade of `LegacyDeployer` and `MultisigLegacyRouter` that is live on Sepolia since 2026-11-19 and scheduled on Ethereum, where it can take effect from 2026-11-21 23:00 UTC. From then on every new legacy gets a version 2 guard, and the owners of an existing legacy get one by deleting it and creating it again. The record, with the evidence and its limits, is `docs/plans/safe-legacy-guard-2026-11.md` in the contracts repository ([pull request 19](https://github.com/10102-io/computing-sc/pull/19)). Like everything deployed since October 2025, this change has no published audit report.

## Editing a legacy

Any Safe owner can initiate an edit (beneficiary changes, allocation changes, name/note, activation trigger). The edit is a normal Safe transaction and requires threshold signatures just like the creation. When it executes:

- The per-legacy contract's state is updated.
- The Router emits the matching update event (`MultisigLegacyConfigUpdated`, `MultisigLegacyBeneficiariesUpdated`, `MultisigLegacyActivationTriggerUpdated` or `MultisigLegacyNameNoteUpdated`); the subgraph updates the entity.
- `SafeGuard.lastTimestampTxs` is updated by the underlying Safe execution, so edits implicitly reset the inactivity timer.

One asymmetry worth naming: **notification settings (watchers, email reminders) are managed by `PremiumSetting`, not by the router.** Until 2026-11-07 only the single EOA that submitted the original creation transaction could edit them. Since the `PremiumSetting` upgrade of that date the legacy's own Safe may edit them too, next to that creator. The app still offers these settings to the creator only; doing it from the Safe means building the Safe transaction by hand for now. Premium, and the timing of reminders, remain the creator's.

## Deleting a legacy

Delete is a Safe transaction at threshold with two calls: `setGuard(0x0)` and `disableModule(legacy contract)`, and `setModuleGuard(0x0)` where the set-up had set it. The app removes only what is this legacy's own: a guard or module guard that is not the legacy's stays. The Safe emits `ChangedGuard` and `DisabledModule`. The Multisig router has no delete function and emits no delete event. The subgraph marks the legacy deleted when it sees the Safe's guard change to anything other than the legacy's own guard. The Safe is left without the 10102 guard and module; the per-legacy contract stays on-chain with no power over the Safe.

Users can also tear down manually via Safe's Transaction Builder at [app.safe.global](https://app.safe.global) (calling `setGuard(0x0)` + `disableModule(...)` on the Safe directly), which is the fallback path if the 10102 UI is ever unavailable.

## Activation: the endgame

Anyone may send the activation (via the app, or via Etherscan using the [Legacy Claim Card](../user-guide/legacy/legacy-claim-card.md)): the caller gains nothing by it, because only the configured beneficiaries become owners. The check is on time alone:

- `block.timestamp - SafeGuard.lastTimestampTxs >= configuredInactivityWindow`.

With the upgrade described under [Guard versions](#guard-versions), the Router asks two more things before it activates any Safe legacy, also one with a version 1 guard:

- the legacy is still set up on its Safe: its guard is the Safe's guard and the legacy an enabled module (`GuardSafeWalletInvalid()`, `ModuleSafeWalletInvalid()` otherwise). Before, a Safe that took the guard off and left the module enabled had a clock that no longer moved;
- a version 2 guard has seen the Safe's last transaction (`GuardBehindSafeWallet()` otherwise; `sync()` on the guard brings it up to date and starts the window again).

`checkActiveLegacy(legacyId)` then answers `false` wherever the activation would revert.

If the checks pass, the Router calls the per-legacy contract, which, as a module the Safe enabled at creation, executes the activation action on behalf of the Safe, bypassing the normal threshold. Its code performs this one action and nothing else.

### Multisig legacy activation

The Module executes `addOwnerWithThreshold(beneficiary_i, newThreshold)` once per beneficiary, then `changeThreshold(configuredThreshold)` to the value the owner specified at creation. The Safe emits `AddedOwner` and `ChangedThreshold` events; the subgraph updates the Safe's owner set and records the legacy as activated. Post-activation, the Safe is now co-owned by the beneficiaries at the new threshold, and everything it holds, stakes, or governs is under their collective control.

### Transfer legacy (Safe owner) activation (retired)

Kept for the record; this flavor was retired in May 2026 and no legacy of it exists on mainnet. The Module executed a series of `transfer` calls (ETH and ERC-20), moving the configured allocations out of the Safe to the beneficiary addresses. The per-legacy contract emits an `Activated` event; the subgraph marks it activated. The Safe itself is untouched (it just becomes lighter by the allocated amounts).

## Why this design

Keeping the Module strictly single-purpose (authorized only for the activation action, never for arbitrary transactions) preserves the security model of the Safe. No matter how badly the Module were compromised, it can only do the one thing the owner explicitly configured at creation. This is the same pattern Safe itself recommends for third-party integrations.
