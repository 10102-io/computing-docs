---
description: >-
  How Transfer legacies work when the owner is a plain EOA (MetaMask, Ledger,
  Trezor, …) with no Safe involvement, and how CREATE2 makes the setup flow
  feel less alarming.
---

# Legacy Contracts Created with EOAs

An **externally owned account (EOA)** is an Ethereum account controlled by a single private key — the wallet most people have in their browser or hardware device. EOAs have no on-chain code, which makes them elegant but creates one genuine constraint for legacy plans: **there is no built-in way for a smart contract to read an EOA's last outgoing transaction timestamp**. The 10102 EOA flow is shaped around that constraint.

This page describes the pure-EOA **Transfer legacy** path. Multisig legacies are Safe-only; Safe-backed Transfer legacies are covered in [Legacy Contracts Created with Safe SDK](legacy-contracts-created-with-safe-sdk.md).

## Contract roles

| Contract | Purpose |
|---|---|
| `TransferEOALegacyRouter` | Creates, edits, deletes, and activates EOA-owner Transfer legacies. Holds the address of the shared `TransferEOALegacy` implementation used by all clones. |
| `LegacyDeployer` | CREATE2 factory. Deploys per-user legacy contracts to a deterministic address. For EOA legacies this deploys an EIP-1167 minimal proxy (~45 bytes of on-chain code) that delegates to the shared implementation. |
| Shared `TransferEOALegacy` implementation | The actual contract logic. One deployment per network; every EOA legacy clone delegates to it. |
| Per-legacy clone | A thin EIP-1167 proxy at a CREATE2-predictable address. Stores beneficiaries, allocations, approved asset list, activation trigger, and last-activity timestamp in its own storage; executes shared code via `DELEGATECALL`. |

Routers are upgradeable behind transparent proxies. Per-legacy clones are _not_ independently upgradeable — each is a fixed, minimal forwarder to whatever implementation the router points at. The router's `_codeAdmin` (a multisig on mainnet) can point new clones at a new shared implementation; existing clones follow the pointer as well, so any implementation swap is a protocol-wide upgrade and not a per-user operation.

## CREATE2 for a better first-time experience

The first time a user creates a legacy, their per-legacy contract does not yet exist on-chain. A naive flow would require the user to (1) deploy the contract, then (2) approve it as a spender for each ERC-20 they want to include. The second step, targeting a freshly-deployed contract, tends to trigger wallet "suspicious transaction" warnings — the approval target has no on-chain history, which is a legitimate heuristic against scams but bad UX for legitimate tooling.

10102 uses `CREATE2` to make the legacy contract's address predictable _before_ deployment. The address is derived from the deployer, a salt (the owner's EOA), and the initcode hash. Because every EOA legacy clone uses the same EIP-1167 initcode (parameterised only by the shared implementation address), the hash is stable across users and the address derivation is deterministic and auditable. This lets us:

- Deploy the contract and approve it in two separate transactions without the approval target looking "unknown" to wallets that track our contract family.
- Show users the legacy address at step 1 and have it match what actually gets deployed.
- Let beneficiaries verify the legacy address on Etherscan against a known pattern (every EOA legacy is a 45-byte EIP-1167 clone pointing at the published shared implementation address — easy to recognise once you've seen one).

## Why clones — the EIP-1167 refactor

Before the clone refactor, each EOA legacy was a full `TransferEOALegacy` deployment: ~6M gas per create, multiple dollars of mainnet fees at typical gas prices. Because every instance's code was byte-identical, paying to redeploy the same bytecode per user was pure waste.

Switching to EIP-1167 keeps the per-user storage isolation — each clone has its own state slots — while dropping the on-chain code to the ~45-byte forwarder stub. `createLegacy` now costs roughly 1.05M gas on mainnet, an 80%+ reduction. The mainnet cutover happened with Legacy #23 (the first clone-based creation); everything before that remains as an independently-deployed contract and continues to function normally.

The trade-off is that "bug in one user's legacy can't corrupt another's" now means _in storage_ — the executable code is shared. In practice the shared implementation is a small, audited contract that does not touch the router's storage and has no mechanism to reach sibling clones, so the isolation guarantee we actually care about (one user's state is untouched by another's activity) is preserved. The `_codeAdmin`-controlled pointer gives us the ability to patch the shared logic if needed, with the honest caveat that any such patch applies to every existing clone at once.

## Creating an EOA legacy

The flow is intentionally split into two explicit signer actions, because a single atomic flow is not possible without EIP-5792 `wallet_sendCalls` support (something we're tracking for a future release).

### Step 1 — Deploy the legacy contract

1. The user configures beneficiaries, allocations, activation trigger (inactivity window in days), and name/note in the UI.
2. The user submits `TransferEOALegacyRouter.createLegacy(...)` with those parameters.
3. The Router calls `LegacyDeployer.deploy(...)` with a deterministic salt. The CREATE2 deploy produces the per-legacy contract at a predictable address.
4. The Router emits `LegacyCreated`. The subgraph creates the legacy entity.
5. The per-legacy contract initializes `lastActivityTimestamp` to the creation block.

### Step 2 — Include assets

A legacy contract holds _rules_, not custody — with one small exception for ETH. For each asset the owner wants included:

- **ERC-20** — the owner submits an `approve(legacyContract, amount)` to the token contract. The subgraph indexes the approval against the legacy. On activation, the Router / per-legacy contract uses `transferFrom` to move tokens from the owner's EOA to beneficiary addresses — _only the pre-approved amount, only on activation_.
- **ETH** — ETH cannot be approved like an ERC-20. The owner swaps ETH for a supported storage token (WETH, a liquid staking token) via the in-app Uniswap integration, then approves the storage token as above. This keeps the contract's permission surface uniform across assets.

The owner can add or remove assets at any time before activation. Removing an asset means approving 0, or deleting and recreating the legacy.

## Heartbeat (reset activation timer)

Because the EOA has no on-chain code to hook into, the per-legacy contract tracks its own `lastActivityTimestamp`. Two things reset it:

- **Explicit heartbeat** — the owner clicks `I'm still alive` in the UI, which submits a tiny transaction to the Router. Cheap, unambiguous, on-chain.
- **Edit** — any edit to the legacy (beneficiaries, allocations, trigger, name) resets the timestamp as a side effect, because the edit is itself an outgoing transaction from the owner's EOA.

_Not_ a heartbeat: arbitrary transactions from the owner's EOA that don't touch the legacy. The per-legacy contract has no way to see those, and there is **no oracle that fills the gap** — activity tracking is exactly "interactions with this legacy," nothing more. This is the deliberate trade-off of the EOA path: a fully on-chain inactivity timer with no external dependency, in exchange for the owner having to check in if they aren't otherwise editing the legacy. See [Indexing & Activity Tracking](indexing-and-activity-tracking.md) for how this compares to the Safe path.

## Editing

Edits are single-EOA transactions, not multi-sig, so they're instant:

- Any field can change — name, beneficiaries, allocations, activation trigger, asset list.
- The Router emits an `LegacyUpdated` event; the subgraph refreshes the entity.
- `lastActivityTimestamp` is reset as a side effect of the outgoing transaction.

There's no notion of "co-signers need to approve the edit" — the whole point of an EOA legacy is that the owner has sole authority.

## Deleting

`TransferEOALegacyRouter.deleteLegacy(...)` does three things atomically at the protocol level, with an optional revoke loop at the UI level:

1. Marks the legacy as deleted in the Router + per-legacy contract. The subgraph updates the entity status.
2. Returns any ETH held _inside_ the per-legacy contract (there's a small amount of dust possible from storage-token mechanics) to the owner.
3. The UI then walks the owner through a sequence of `approve(legacyContract, 0)` calls — one per previously-approved token — to revoke spender permissions. This is best-effort; a partial success still leaves the legacy deleted.

Empty legacies (no tokens ever approved) skip the revoke loop entirely. The revoke loop is strictly a "for your protection" cleanup; the deleted legacy contract can no longer call `transferFrom` anyway, but dangling approvals are a general hygiene problem worth fixing.

## Activation

Activation is a single transaction any beneficiary can submit:

1. Beneficiary calls `TransferEOALegacyRouter.activeLegacy(legacyId, ...)` from an address matching one of the configured beneficiaries (primary, or a contingent whose window has elapsed).
2. The per-legacy contract evaluates the activation trigger entirely on-chain — `_lastTimestamp + lackOfOutgoingTxRange <= block.timestamp` — with no external call. There is no oracle and no off-chain activity lookup: the only "activity" the contract considers is the owner's interactions with this legacy (see [Heartbeat](#heartbeat-reset-activation-timer) above). If the owner hasn't touched the legacy for the configured window, the claim is allowed; otherwise it reverts.
3. If the check passes, the Router iterates the allocations and issues `transferFrom(owner, beneficiary_i, amount_i)` for each (asset, beneficiary) pair.
4. The Router emits an `Activated` event; the subgraph marks the legacy activated.

Batches over 100 transfers are executed in chunks, with the remaining distribution callable via a `Claim Remaining Fund` follow-up — see the user guide for the beneficiary-side flow.

## No external dependency at activation

EOA activation has no off-chain dependency: the eligibility decision is a single on-chain comparison inside the per-legacy contract, so it works even if our app, backend, or indexer are entirely down. The flip side is that the inactivity timer is only ever reset by the owner's own interactions with the legacy — there is no oracle watching the owner's broader wallet activity. An owner who is active elsewhere but never touches the legacy will still see the timer count down, which is why the [heartbeat](#heartbeat-reset-activation-timer) exists. Safe-owned legacies get a softer version of this: their Safe Guard records activity on every Safe transaction, so normal wallet use keeps the legacy fresh automatically.

## Why split EOA and Safe paths at the contract level

The two flows share 80% of their semantics, but the 20% that differs — how activity is tracked, who can authorize edits, how activation executes — is different enough that conflating them would force both paths to carry the other's complexity. Two thin routers with clear contracts are easier to audit and easier to upgrade in isolation.
