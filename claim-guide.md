---
description: >-
  For the person holding a printed Legacy Claim Card: how to claim with a web
  browser and a wallet, step by step, without the 10102 app.
---

# Claim Guide

This page is for someone who was handed a **Legacy Claim Card** and now needs to use it. The card carries the short steps. This is the long form, written for a reader who has never used a block explorer.

Nothing here needs the 10102 app or website. The legacy is a contract on Ethereum, and the steps below call it through a public block explorer. If the app is reachable, it is the easier way: see [Activate a Legacy Contract and Claim Funds](user-guide/legacy/activate-a-legacy-contract-and-claim-funds.md).

{% hint style="info" %}
**Nobody needs your private key to help you claim.** 10102 never asks for a private key or a recovery phrase, by email, by phone or on any page. Anyone who does is not us.
{% endhint %}

## What you should have

- **The claim card** (green stripe at the top). It names the claim contract (the router), the Legacy ID, the network, the type of legacy and, for a wallet legacy, the token addresses.
- **The private-key sheet** (blue stripe), only if the owner created a wallet for you. If the beneficiary address on the card is a wallet you already control, there is no such sheet and you do not need one.

The **TYPE** line on the card says which part of this page is yours:

| The card says | Read |
|---|---|
| Transfer legacy (EOA wallet) | [Wallet legacy](#wallet-legacy) |
| Multisig legacy (Safe co-signers) | [Multisig legacy](#multisig-legacy) |

## Check the addresses

On Ethereum Mainnet (chain ID 1) the claim contracts are:

| Legacy type | Claim contract (router) |
|---|---|
| Wallet legacy | `0x4E81E1Ed3F6684EB948F8956b8787967b1a6275b` |
| Multisig legacy | `0x7c7bf503DF70eBE3520f65Cc0Ff1aF093Fa85038` |

Do not take this page's word alone. The same addresses are in the file `contract-addresses.json` of the public repository [github.com/10102-io/computing-sc](https://github.com/10102-io/computing-sc), and the block explorer shows each as a verified contract. A card made on another network names that network's router: the card is right for its own network.

The claim functions are on the **router**. The card also prints the legacy's own contract address, for the record: calling a claim function there fails with `OnlyRouter`.

## Wallet legacy

A wallet legacy moves the owner's tokens from the owner's wallet to the beneficiaries, by the shares the owner set.

### Step 1: check that it can be claimed

1. Open `https://etherscan.io/address/0x4E81E1Ed3F6684EB948F8956b8787967b1a6275b` in a web browser.
2. Open the **Contract** tab, then **Read as Proxy** (if you only see **Read Contract**, use that).
3. Find `checkActiveLegacy`, type the **Legacy ID** from the card and press **Query**.

- `true`: the owner's inactivity period has passed. Go on to step 2.
- `false`: not yet. The owner is, or recently was, active. Wait and check again later. There is nothing else to do in the meantime.
- `LegacyNotFound`: the Legacy ID is wrong, or the explorer is on another network than the card.

### Step 2: get the wallet ready

- With a private-key sheet: install a wallet app you control yourself (MetaMask or Rabby, for example), choose to import an account by private key, and scan or paste what the sheet's code contains. The address the wallet then shows should equal the beneficiary address on the card.
- Without one: use the wallet whose address is on the card.
- The wallet needs a little ETH for the network fee.

### Step 3: claim

1. On the same explorer page open **Write as Proxy** (or **Write Contract**) and connect the wallet.
2. Find `activeLegacy` and fill in:
   - `legacyId_`: the Legacy ID from the card.
   - `assets_`: the token addresses from the card, separated by commas, inside square brackets, for example `[0xA0b8...eB48,0xdAC1...1ec7]`.
   - `isETH_`: `true`. It also sends any ETH the legacy itself holds.
3. Press **Write**, confirm in the wallet and wait for the transaction.

**What arrives.** Your share of each token named in the call, sent from the owner's wallet to the beneficiary wallet in that same transaction. The other beneficiaries of the same line receive theirs in it too. A legacy created since 2026-09-29 carries a claim fee, fixed when it was created (0.25% for legacies created today, never more than 0.5%), taken from what is delivered. You receive each token as it is and can convert it later.

**The token list on the card can be out of date.** It is the list on the day the card was made. A token the owner added later is not lost: send another claim that names its address.

**Many tokens.** One transaction moves up to 100 transfers. If something is still missing after a claim that went through, send the same claim again.

### ETH instead of a stored token (optional)

Only if you are the only beneficiary of your line and the owner stored ETH as a token: `activeLegacyAndUnswap(legacyId_, assets_, amountOutMin_, deadline_)` converts it in the same transaction. `amountOutMin_` is the least ETH you accept, in wei, and must be above zero. `deadline_` is a Unix timestamp about an hour ahead: search for "unix timestamp now" and add 3600. If this is unfamiliar, use step 3: nothing is lost by it.

### Legacies created since 2026-11-13

A wallet legacy of the new version can give each heir backups and can hand on NFTs. With backups on, each heir claims their own share, and a backup claims only when their turn has come. NFTs are claimed on the legacy contract itself, with its `claimNfts` function. Both are described in [Activate a Legacy Contract and Claim Funds](user-guide/legacy/activate-a-legacy-contract-and-claim-funds.md#the-heir-page-new-version).

### If the transaction fails

| The explorer shows | What it means | What to do |
|---|---|---|
| `NotEnoughContitionalActive` | The inactivity period has not passed. | Repeat step 1 and wait for `true`. |
| `CannotClaim` | Your line is not reachable yet: you are a backup, and the people before you still have their turn. | Wait and try again later. |
| `OnlyBeneficaries` | The connected wallet is not a beneficiary of this legacy. | Connect the wallet whose address is on the card. |
| `LegacyNotFound` | Wrong Legacy ID, or wrong network. | Check the card and the network the explorer shows. |
| `NumAssetsInvalid` | No token address was given and `isETH_` was `false`. | Give the token addresses, or set `isETH_` to `true`. |
| `ZeroAmountOutMin`, `UnswapNeedsSoleRecipient` | The conversion to ETH is not available to you. | Use step 3. |
| `OnlyRouter` | The function was called on the legacy contract. | Call it on the router, as in step 1. |

### Checks that need no wallet

On the **legacy contract** address from the card, under **Read Contract**:

- `getTriggerActivationTimestamp()`: the three Unix timestamps from which the first people, the first backup and the second backup can claim.
- `getBeneficiaryLayer(address)`: `1`, `2` or `3` for the line an address belongs to, `0` if it is not a beneficiary.
- `getLegacyBeneficiaries()`: the addresses named in the legacy.

Any "unix timestamp converter" turns a timestamp into a date.

## Multisig legacy

A multisig legacy does not send money to you. When it is claimed, every beneficiary becomes a co-signer of the owner's Safe account, with the number of signatures the owner chose. From then on the beneficiaries manage what the Safe holds together.

The Safe records the time of its last transaction. The legacy can be claimed once the owner's inactivity period has passed since then. Every transaction of the Safe starts the period again.

### Step 1: check that it can be claimed

1. Open `https://etherscan.io/address/0x7c7bf503DF70eBE3520f65Cc0Ff1aF093Fa85038` in a web browser.
2. Open the **Contract** tab, then **Read as Proxy** (or **Read Contract**).
3. Find `checkActiveLegacy`, type the **Legacy ID** from the card and press **Query**.

- `true`: it can be claimed. Go on to step 2.
- `false`: it cannot be claimed now. Most often the Safe was active too recently: wait and check again later. If the answer stays `false` long after the period should have passed, see the next section.
- `LegacyNotFound`: wrong Legacy ID, or wrong network.

### If the answer stays `false`

Since an upgrade of the router, live on Ethereum since 2026-11-21, it answers `false` in two more cases, and each can be read from the chain without a wallet.

1. On the router's page, `legacyAddresses(legacyId)` gives the legacy contract and `guardAddresses(legacyId)` the guard contract. On the legacy contract, `getLegacyOwner()` gives the Safe.
2. **Is the legacy still connected to its Safe?** Open the Safe's address on the explorer, **Read as Proxy**, and call `isModuleEnabled` with the legacy contract's address. `false` means the owners of the Safe disconnected the legacy: it cannot be claimed, and only they can connect it again. The same holds when the Safe no longer runs the legacy's guard; `app.safe.global` shows a Safe's guard and modules under Settings, Modules, to anyone who opens the Safe by its address.
3. **Has the guard counted the Safe's last transaction?** Open the guard contract's page. If it lists `lastNonce` and `sync` (legacies created after the upgrade), compare `lastNonce()` with `nonce()` on the Safe. If the two numbers differ, the Safe made a transaction the guard did not see. Anyone can have it counted: under **Write Contract** on the guard, send `sync`. The inactivity period starts again from that moment, so wait it out and repeat step 1. If the guard lists neither function, it has nothing to count and this case does not apply.

If the legacy is connected, the guard is up to date and the answer is still `false`, the period has not passed: `getLastTimestampTxs()` on the guard is the time it runs from.

### Step 2: claim

1. On the same page open **Write as Proxy** (or **Write Contract**) and connect a wallet that holds a little ETH for the network fee. It does not have to be a beneficiary's wallet: anyone may send this transaction once the period has passed.
2. Find `activeLegacy`, type the Legacy ID, press **Write** and confirm.

**What happens.** In one transaction every beneficiary is added as an owner of the Safe, and the Safe's threshold is set to the number of signatures the legacy's owner chose.

### Step 3: open the Safe

1. Go to `https://app.safe.global`.
2. Connect your beneficiary wallet. If the owner created it for you, import the private-key sheet into a wallet app first.
3. Choose Ethereum and open the **Safe account** address from the card. You are a co-signer: you can propose a transaction, and it goes through when enough co-signers have approved it.

If the Safe address was left off the card at the owner's request, read it from the legacy contract: open the legacy contract address from the card on the explorer and call `getLegacyOwner()`.

### If the transaction fails

| The explorer shows | What it means | What to do |
|---|---|---|
| `NotEnoughContitionalActive` | The inactivity period has not passed: a Safe transaction started it again. | Repeat step 1 and wait for `true`. |
| `GuardBehindSafeWallet` | The Safe made a transaction its guard has not counted. | Send `sync` on the guard (see "If the answer stays `false`"), then wait out the period. |
| `GuardSafeWalletInvalid` or `ModuleSafeWalletInvalid` | The legacy is no longer connected to its Safe: the owners removed its guard or disabled it. | This path is closed unless the owners connect it again. Speak to whoever handles the owner's affairs. |
| `LegacyNotActive` | The legacy was already claimed, or the owner removed it. | Go to step 3 and look at the Safe's owners: you may already be a co-signer. |
| `LegacyNotFound` | Wrong Legacy ID, or wrong network. | Check the card and the network the explorer shows. |
| `ExecTransactionFromModuleFailed` | The Safe no longer accepts the legacy: the owner disconnected it. | This path is closed. Speak to whoever handles the owner's affairs. |
| `OnlyRouter` | The function was called on the legacy contract. | Call it on the router, as in step 1. |

### Checks that need no wallet

- On the router: `legacyAddresses(legacyId)` and `guardAddresses(legacyId)` give the legacy contract and the guard contract for your Legacy ID.
- On the legacy contract: `getBeneficiaries()` lists who becomes a co-signer, `getMinRequiredSignatures()` the threshold after the claim, `getTriggerActivationTimestamp()` the time from which it can be claimed.
- On the guard contract: `getLastTimestampTxs()` is the time of the Safe's last transaction. On a guard of a legacy created after the November 2026 upgrade, `lastNonce()` is the Safe's transaction count as the guard last saw it.

## Where this page is kept

The address printed on the card is `docs.10102.io/claim-guide`. The same text is a plain file, `claim-guide.md`, in the public repository [github.com/10102-io/computing-docs](https://github.com/10102-io/computing-docs), so it can be read there if this site is ever unreachable.

## See also

- [Legacy Claim Card](user-guide/legacy/legacy-claim-card.md): what the card is and when the owner should print it again.
- [Activate a Legacy Contract and Claim Funds](user-guide/legacy/activate-a-legacy-contract-and-claim-funds.md): the same claim through the app.
