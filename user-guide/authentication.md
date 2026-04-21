---
description: >-
  Connect and disconnect wallets. We support every common Ethereum wallet via
  standard connectors — WalletConnect, Ledger, Trezor, MetaMask, and more.
---

# Authentication

10102 Computing Legacy never takes custody of your funds and never asks you to create a 10102-specific account. Your wallet _is_ your identity. Connecting a wallet is a read-only signature handshake; signing in does not move anything.

## Supported wallets

Any wallet that speaks one of the standard Ethereum connector protocols works. That includes, but isn't limited to:

- **MetaMask** — browser extension or mobile app.
- **WalletConnect** — protocol covering most mobile wallets (Rainbow, Trust Wallet, 1inch Wallet, imToken, …).
- **Coinbase Wallet** — extension, mobile, and smart wallets (passkeys).
- **Hardware wallets** — Ledger and Trezor, either directly over USB or through their respective bridge apps (Ledger Live, Trezor Suite).
- **Rainbow** — extension and mobile.

If your wallet exposes a standard provider, it'll show up in the Connect modal automatically. The app treats every connector identically once connected — there's no special case for any brand.

## Connect

1. Click **Connect Wallet** at the top right of the app.
2. Choose your wallet from the modal.
3. Complete the connection flow in your wallet's UI:
   - Extensions open a popup asking for approval.
   - Mobile wallets show a QR code; scan it in the wallet app.
   - Hardware wallets may require you to unlock the device and open the Ethereum app.
4. Back in 10102, you'll see your wallet address (and any ENS name it resolves to) at the top right.

{% hint style="info" %}
**Don't have a wallet installed?** The Connect modal links out to each wallet's official install page. We never redirect through third-party download sites — only to the wallet vendors themselves.
{% endhint %}

## Switch wallets

Click your wallet address in the top right, then **Disconnect**. Reconnect with a different wallet via the standard flow.

Some wallets (MetaMask, most extensions) also let you switch accounts inside the wallet itself without disconnecting — the app picks up the change automatically.

## Disconnect

Click your wallet address in the top right, then **Disconnect**. You'll be returned to the Connect screen. No data is persisted client-side across a disconnect, other than standard browser storage (theme, UI preferences).

## Networks

- **Mainnet** (Ethereum mainnet) is the canonical deployment.
- **Sepolia** testnet is available for kicking the tires without real funds — useful for trying a flow end-to-end (creation, activation, claim) with throwaway ETH.

Switch networks in your wallet; the app follows. All feature parity between mainnet and Sepolia — everything you can do on one, you can do on the other.

## Signing vs. sending

Two different things happen with your wallet while you use the app:

- **Signing a message** — a pure cryptographic signature; doesn't send a transaction, doesn't cost gas, doesn't touch the blockchain. Used rarely, mostly for opt-in features that need to prove you control an address.
- **Sending a transaction** — a real Ethereum transaction that costs gas and changes on-chain state. Used for creating/editing/deleting legacies and timelocks, approvals, activations, and claims.

Your wallet clearly distinguishes the two in its prompts. If you're ever uncertain about a wallet prompt, the details page of whatever you're working on will describe the transaction you're expected to sign next — compare it against what your wallet shows.

## See also

- [Concepts](./concepts.md)
- [Create a Legacy Contract](./legacy/create-a-legacy-contract.md)
