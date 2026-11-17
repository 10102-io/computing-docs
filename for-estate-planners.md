---
description: >-
  For estate planners, attorneys and advisors: how clients include digital
  assets in their plan with 10102, what you see as their authorized
  watcher, and the partner program.
---

# For Estate Planners

Digital assets held on a blockchain are the one asset class that most
estate plans cannot reach: if the keys are lost with the owner, a will
cannot recover them, and handing the keys to someone else in advance is a
custody risk. 10102 closes that gap
without anyone holding the keys, you and us included.

Start here: [app.10102.io/advisor](https://app.10102.io/advisor?from=advisor).

## What your client sets up

Your client creates a **legacy contract** from their own wallet: who
receives what and in which shares, optional backup people, and how long a
silence triggers it. They check in from time to time (or it renews itself
from their normal wallet activity, with Premium). If they go silent past the
period they chose, the people they named can claim, including with a
printed claim card that needs no wallet experience.

Nothing is custodial: the assets stay in the client's wallet until a claim
happens, and they only ever go to the people the client named. See
[Create a Legacy Contract](user-guide/legacy/create-a-legacy-contract.md)
and [Legacy Claim Card](user-guide/legacy/legacy-claim-card.md).

For holdings they want protected against theft as well, the
[Shield Vault](user-guide/shield.md) adds a waiting period on every
withdrawal and a printed recovery sheet.

## What a legacy can express since November 2026

Wallet legacies created since 2026-11-13 run a new version of the contract.
Three additions answer questions planners asked us; the last two are part
of Premium, and each is set on the legacy's own page after it exists:

- **NFTs.** A client can name single NFTs, each for one beneficiary. Only
  collections on a short list can be named: on Ethereum, since 2026-11-16,
  CryptoPunks (in the ERC-721 wrapper "CryptoPunks 721"), Bored Ape Yacht
  Club and Pudgy Penguins. The list can change, and being on it is neither
  an endorsement nor a valuation. An NFT stays in the client's wallet and
  can be claimed only if the wallet still holds it then.
- **Backup beneficiaries, per share.** Each beneficiary can have up to two
  backups for their own share. If the beneficiary has not claimed a wait
  after the claim date (the client chooses it, from 30 days to 10 years),
  the first backup can claim that share, and the second one more wait
  later. No one checks why a share was not claimed: once the wait has
  passed, the backup can claim, and a claim cannot be undone.
- **An executor.** One address the client trusts, which could be yours,
  that can confirm on-chain that the client has died, but only after the
  client has been silent for half their check-in period. Claims then open
  30 days later if that is sooner than the normal date, and any check-in by
  the client cancels it. The executor never receives anything, cannot
  claim, and cannot be a beneficiary or a backup.

Three things to tell a client plainly:

- **Nobody verifies an executor's confirmation**, neither the contract nor
  10102. It is a role in the smart contract only, and it appoints no
  executor of the estate under any law. Name only someone trusted.
- **The delivery fee.** When beneficiaries claim tokens or ETH, 0.25% is
  deducted from what they receive, at the rate fixed into the legacy when
  it was created and shown before the client confirmed. NFTs carry none.
- **This version has not had an outside audit.** The legacy contracts were
  audited twice (reports of January 2025 and October 2025); the version in use since 2026-11-13 was reviewed
  internally and went live before an external review, which is still to be
  commissioned. A legacy cannot be upgraded once created.

A legacy created before 2026-11-13 keeps working as it is. The app offers a
guided move to the new version, which removes the old legacy and creates a
new one: a new contract address (claim cards must be printed again and any
reference in your documents updated), and the check-in timer starts again.

## What you see as their watcher

Your client adds your wallet address as an **authorized watcher** in
Settings, Authorized watchers (part of their Premium plan; you pay nothing).
They choose the visibility:

- **Limited** (the default): no addresses. The contract is identified by a
  private identifier your documents can reference.
- **Full**: every address, so you can look the contract up on Etherscan.

Connect that wallet at [app.10102.io/advisor](https://app.10102.io/advisor?from=advisor)
and every client contract you watch appears in one read-only list, most
urgent first: its status (Active, Check-in due soon, Ready to activate,
Activated), the days left to the next check-in, and the allocations at the
visibility you were given. Details in
[Manage Authorized Watchers](user-guide/premium-features/manage-authorized-watchers.md).

## What you never control

A watcher can never check in, edit, delete, activate, move or redirect
anything. This is deliberate: the legacy still works if the owner has died,
if you are unreachable, and if 10102 no longer exists, because nothing in a
claim depends on a third party. You carry no key and no custody.

Being named as a client's executor is a different thing from being their
watcher: it is a separate choice the client makes, with a separate address
if they wish, and it gives exactly one power, described above.

## Tying it to your documents

- Reference the contract in the will or letter of wishes by the identifier
  the Advisor view shows (limited visibility) or by its contract address
  (full visibility).
- Keep the printed claim cards and recovery sheets with the estate papers,
  in the same way as any other instrument of access.
- The legal effect of a legacy contract, and how it interacts with the
  client's will and local succession law, is your professional judgement:
  10102 moves assets according to the contract, not according to a court.

## Partner program

Firms and advisors who bring clients can join the **advisor tier** of the
[Partner Program](partners.md): a recurring share of your clients' Premium
renewals, attributed on-chain so every statement can be checked on
Etherscan. If your profession bars referral fees (for example French
avocats and notaires), **discount mode** gives your clients a lower Premium
price instead, and you receive nothing. Premium is $99 per year or $399
once, paid on-chain; your code can be a coupon the client types at
checkout.

To start, write to [info@10102.io](mailto:info@10102.io?subject=Advisor%20partner)
with your firm, the partner code you would like, and a USDC payout address
(or "discount mode").
