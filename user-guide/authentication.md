---
description: >-
  Connect and disconnect wallets, and sign in on each device so your alerts
  and names follow your wallet. We support every common Ethereum wallet via
  standard connectors: WalletConnect, Ledger, Trezor, MetaMask, and more.
---

# Authentication

10102 Computing Legacy never takes custody of your funds and never asks you to create a 10102-specific account. Your wallet _is_ your identity. Connecting a wallet is a read-only signature handshake; signing in does not move anything.

## Supported wallets

Any wallet that speaks one of the standard Ethereum connector protocols works. That includes, but isn't limited to:

- **MetaMask**: browser extension or mobile app.
- **WalletConnect**: protocol covering most mobile wallets (Rainbow, Trust Wallet, 1inch Wallet, imToken, …).
- **Coinbase Wallet**: extension, mobile, and smart wallets (passkeys).
- **Hardware wallets**: Ledger and Trezor, either directly over USB or through their respective bridge apps (Ledger Live, Trezor Suite).
- **Rainbow**: extension and mobile.

If your wallet exposes a standard provider, it'll show up in the Connect modal automatically. The app treats every connector identically once connected; there's no special case for any brand.

## Connect

1. Click **Connect Wallet** at the top right of the app.
2. Choose your wallet from the modal.
3. Complete the connection flow in your wallet's UI:
   - Extensions open a popup asking for approval.
   - Mobile wallets show a QR code; scan it in the wallet app.
   - Hardware wallets may require you to unlock the device and open the Ethereum app.
4. Back in 10102, you'll see your wallet address (and any ENS name it resolves to) at the top right.

{% hint style="info" %}
**Don't have a wallet installed?** The Connect modal links out to each wallet's official install page. We never redirect through third-party download sites, only to the wallet vendors themselves.
{% endhint %}

## Switch wallets

Click your wallet address in the top right, then **Disconnect**. Reconnect with a different wallet via the standard flow.

Some wallets (MetaMask, most extensions) also let you switch accounts inside the wallet itself without disconnecting; the app picks up the change automatically.

## Disconnect

Click your wallet address in the top right, then **Disconnect**. You'll be returned to the Connect screen. Disconnecting also signs this device out (see below). What the app keeps in this browser, such as the names you typed, your notes and your watch list, stays here until you clear the browser's storage.

## Sign in on each device

Some things in 10102 belong to your wallet rather than to one browser: your wallet alerts, and the names you give your legacies and beneficiaries. To see them on your phone and your laptop alike, sign in once on each device.

1. Open **Settings**, **Devices**. The same **Sign in on this device** button also appears next to wallet alerts and in Assets elsewhere on Home.
2. Sign one message. It is not a transaction: it costs nothing and puts nothing on the blockchain. Your wallet shows: "Sign in to 10102 on this device. This is not a transaction and costs nothing."

After that, on this device:

- **Wallet alerts**: Settings and Home list every alert of this wallet, including ones you set on another device, and you can change the email of each one or turn it off. See [Wallet alerts](./wallet-alerts.md#on-every-device).
- **Names**: the names you gave legacies and beneficiaries show here, and the ones typed here show on your other devices. They are stored encrypted on our server and shown only to this wallet, signed in. Where your wallet already has a name for the same item, that name is kept; after that, the latest change from any device wins.

**What stays on the device.** Notes to beneficiaries, your watch list, amounts, and the secrets behind a gift link or card never leave this browser through sign-in. Only names do.

**A copied sign-in does not work elsewhere.** Each browser makes its own key, which it cannot export, and signs every request with it. A copied sign-in message, or a stolen session, is useless without that key.

**How long it lasts.** 7 days for an ordinary wallet, 24 hours for a Safe or another smart account. It ends sooner if you disconnect or switch wallet. Settings, Devices shows until when.

**Sign out.** Settings, Devices has **Sign out on this device** and **Sign out on every device**, for a lost phone or a shared computer.

**Delete my names.** **Delete my names from 10102** deletes the names stored for your wallet, stops syncing them, and signs this wallet out on every device. Each device keeps the names typed on it. You can turn syncing on again later.

{% hint style="info" %}
Sign-in is optional. Without it, everything works as before on this device: legacies, timelocks and claims never depend on it. Cypherpunk Mode hides it, like every email feature.
{% endhint %}

## Networks

- **Mainnet** (Ethereum mainnet) is the canonical deployment.
- **Sepolia** is the test network, for trying legacies and timelocks without real funds. It's useful for trying a flow end-to-end (creation, activation, claim) with throwaway ETH: activation periods there are counted in minutes instead of months.

Switch networks in your wallet; the app follows. A link ending in `?network=testnet` opens the app on Sepolia.

Sepolia is not a full copy of mainnet. It is for the two foundations, Legacy and Timelock. On any network but mainnet, a band above the header says it is a test network, with **Switch to Ethereum**, and the menu keeps only Home, Legacy and Timelock (plus Pricing). The products built on legacies and timelocks run on Ethereum mainnet only:

- the Shield Vault and Stake & Shield,
- Save, and Save on autopilot,
- Heirloom,
- the email wallet.

They leave the menu and Home on Sepolia, and their pages say "(product) runs on Ethereum", with **Switch to Ethereum** and **Back to Home**. The Shield Vault, Stake & Shield, Save and Heirloom have test deployments, so their page also offers **Open the test version**; Save on autopilot and the email wallet have none. Pages for acting on something that already exists stay open on every network: a gift's claim page, a Shield Vault holding's status page and its printed-card claim, the Save on autopilot stop link, and the links in wallet alert emails. Wallet alerts themselves work on mainnet only.

If the default Ethereum node providers are slow for you, or you'd rather the app read chain state through your own endpoint, see [Bring your own RPC](./bring-your-own-rpc.md).

## Signing vs. sending

Two different things happen with your wallet while you use the app:

- **Signing a message**: a pure cryptographic signature; doesn't send a transaction, doesn't cost gas, doesn't touch the blockchain. Used rarely, mostly for opt-in features that need to prove you control an address.
- **Sending a transaction**: a real Ethereum transaction that costs gas and changes on-chain state. Used for creating/editing/deleting legacies and timelocks, approvals, activations, and claims.

Your wallet clearly distinguishes the two in its prompts. If you're ever uncertain about a wallet prompt, the details page of whatever you're working on will describe the transaction you're expected to sign next; compare it against what your wallet shows.

## See also

- [Concepts](./concepts.md)
- [Create a Legacy Contract](./legacy/create-a-legacy-contract.md)
