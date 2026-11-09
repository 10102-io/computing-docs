---
description: >-
  An email when something leaves your wallet, or could, that you might not
  see: tokens taken with a permission, a new permission, a contract your
  wallet now runs, a transaction you did not send. Free for your own
  wallet. Bitcoin addresses in your watch list.
---

# Wallet alerts and the Bitcoin watch

Wallet alerts email you when something leaves a wallet, or could, that you
might not notice in time: tokens taken through a permission you gave long
ago, a new permission to move your tokens, a contract your wallet now
runs, or a transaction you did not send. Block explorers already email
plain transfers; these alerts cover what they do not explain. They watch
what leaves, not what arrives: incoming ETH and NFTs are not covered, on
purpose, because that is where spam and fake tokens live.

They work on Ethereum mainnet. Open them from **Settings**, **Wallet
alerts** (`app.10102.io/settings?tab=alerts`).

## Free and Premium

- **Free:** alerts for the wallet you are connected with.
- **Premium:** alerts for up to 20 other wallets and for Bitcoin addresses.
  You add them from **Assets elsewhere** on Home, next to each watched
  address. If Premium ends, these alerts pause and show as paused until you
  renew; the free alert for your own wallet keeps running.

## Turning alerts on

1. **Type your email.** Under the field the page shows a short code made
   from your email, for example `0x3f2a…9c1d`.
2. **Sign once.** No transaction, no network fee, nothing on the
   blockchain. Your wallet shows the code, not your email: check that its
   first and last characters match the page.
3. **Confirm your email.** We email a confirmation link, valid for 7 days.
   Nothing else is sent to that address until you confirm. If the request
   was not yours, the "Not you?" link in that email deletes it.

Optional: **Also email me when it receives known tokens** adds an email for
incoming transfers of tokens we recognise. For a Bitcoin address the box
reads **Also email me when it receives 0.0001 BTC or more** (confirmed
receipts only). Bitcoin alerts set up before 2026-11-09 send spends only
until you save them again with the box ticked.

If the email address looks like a typo of a common provider, the page
suggests the fix before and after you submit.

## On every device

Alerts belong to your wallet, not to one browser. Once you
[sign in on the device](authentication.md#sign-in-on-each-device) (one
signature, no transaction), **Settings**, **Wallet alerts** and **Assets
elsewhere** on Home list every alert of this wallet, including ones you set
on another device. You can change the email of each one or turn it off
(**Change email** and **Turn off** next to other addresses). On Home,
an address with alerts that is not in this device's watch list shows under
"addresses with alerts are not in this list", with **Alerts** to change or
turn it off, and **Watch** to add it to the list.

Without signing in, a device lists only the alerts it set up itself.

## What each alert means

Each email lists the transactions with a link to Etherscan (mempool.space
for Bitcoin); for a warning it also names who sent the transaction and the
contract it called. The most serious event of the batch is the subject.

| Alert | What happened | What to do |
|---|---|---|
| **Tokens left this wallet** | Tokens left in a transaction your wallet did not send. Someone used a permission it gave earlier: an approval or a signed permit. | If you do not know why, remove the permissions you do not trust (the email links to Etherscan's approval checker) and move what is left to a safe wallet. |
| **A signed permit gave a new permission** | A permission to move your tokens was granted with your signature, and someone else submitted it. | If you did not sign it, remove the permission now. |
| **New permission to move your tokens** | Your wallet gave an approval: a token, an NFT, a whole NFT collection, or Permit2. | If it was not you, remove the permission now. |
| **A Safe module moved tokens** | For a Safe: a module moved funds without the owners signing. | If you did not add the module, remove it. |
| **Now runs an unknown contract's code**, **is now a smart account**, **is a plain wallet again** | Your wallet signed an EIP-7702 delegation: it now runs a contract's code, changed it, or stopped. Whoever controls that code can move everything in the wallet. A known wallet's smart account is named (MetaMask, Uniswap Wallet, Ambire, Alchemy, Coinbase Smart Wallet and others); anything else is a warning. Coinbase's proxy counts as known only with Coinbase's own code behind it. | If you just upgraded your wallet to a smart account, nothing. If not, move what is left to a new wallet now: this is a common phishing step. |
| **Someone new can act for this wallet**, **who can act for it changed** | For a known smart account: an owner, signer, key or module was added or removed, including when it was first set up. The wallet itself as owner is the usual setup and reads as information. | If you did not add it, it can act for your wallet: move what is left to a new wallet now. |
| **ETH left this wallet without a transaction from it** | For a wallet running a contract's code: ETH left while the wallet sent nothing. Only that code can do this. Limit: ETH that arrived in between can hide some or all of it. | If you did not expect it, move what is left to a new wallet. |
| **This wallet sent a transaction** | Your wallet (or your Safe) sent one or more transactions, including operations a smart account sends through a bundler (ERC-4337), whose transfers count as the wallet's own. A delegation someone else submitted for you is reported as the delegation alert above, not as a send. | If it was you, nothing. If not, someone may have your key: move what is left to a safe wallet. |
| **Your legacy moved tokens** | Your legacy paid the people you named, because the time you set has passed. | If you did not expect it, open your legacy. |
| **Permission for your 10102 contract** | You gave your own legacy, timelock, Shield Vault or Premium plan permission to move tokens. | Nothing: this is how those contracts work. |
| **Tokens received** | Known tokens arrived (only if you asked for it). | Nothing. |
| **Bitcoin sent** | The watched Bitcoin address is spending, as soon as the transaction is seen, before it confirms (the email says "unconfirmed"). No second email when it confirms. | If it was not you, the key is exposed: move what is left to a new address. |
| **Bitcoin received** | A confirmed receipt of 0.0001 BTC or more (only if you asked for it). Smaller amounts are ignored: tiny receipts are how dust attacks track an address. | Nothing. |

Fake tokens can put any address in your history (address poisoning). An
event from a token contract we do not recognise never raises an alert on
its own; the email only counts them and reminds you never to copy an
address from your history.

**How often.** We read new blocks every minute and a half, two blocks
behind the chain's head so a reorganised block cannot raise a false alert,
so an alert arrives within minutes. One email covers
everything new for an address, at least 5 minutes apart and at most 12 a
day; the 12th says it is the last for that day.

## Stopping alerts

- **From any alert email:** the stop link at the bottom turns alerts off
  for that address and deletes your email for it, in one click. We then
  send one last email saying alerts are off, so a mail filter that follows
  links cannot silence them without you seeing it.
- **In the app:** **Turn alerts off and delete the email** under your
  wallet's alert, or **Turn off** next to any other address in Settings,
  Wallet alerts. One signature, no transaction. Signed in on the device,
  this works for alerts set on any device.

## The Bitcoin watch

In **Assets elsewhere** on Home, **Add a watch** also takes a Bitcoin
address: `1…`, `3…`, `bc1q…` or `bc1p…`, with its checksum checked. The
card shows its confirmed balance in dollars, labelled watch-only.

- **Read by our server.** The balance comes from mempool.space through our
  server, so the lookup never comes from your browser.
- **One address.** Bitcoin wallets that change their receive address spread
  a balance over many addresses; the watch shows only the one you added.
- **Watch-only.** A legacy covers Ethereum assets only, so a Bitcoin
  address counts in what you watch, never in what a legacy can deliver.

The watch list itself stays in your browser, like every watched wallet. You
can back it up encrypted with one wallet signature to restore it on another
device.

## Privacy

- **Nothing on the blockchain.** Turning alerts on or off is a signature,
  never a transaction.
- **The signature carries a fingerprint, not your email.** The code your
  wallet shows is a hash of your email (trimmed and lowercased). Our server
  checks that it matches the email you typed.
- **Encrypted, and only while an alert is on.** We store the email and the
  watched address encrypted, one key per record, and your wallet address
  as a keyed hash next to its encrypted copy. Turning alerts off deletes
  the record.
- **No watch list on our side.** We keep no list of what you watch, except
  the encrypted backup if you make one. A watched Bitcoin address passes
  through our server only to read its balance, kept in memory for a minute
  and not stored.

{% hint style="info" %}
**Honest limits.** An alert warns you; it cannot stop a transaction. To make
a stolen key unable to take a holding, use the
[Shield Vault](shield.md), where every withdrawal waits in public and your
recovery sheet can stop it. Alerts depend on our server and our email
provider: if an email is late or lost, nothing changes on the blockchain,
and your legacy, timelocks and claims never depend on it.
{% endhint %}
