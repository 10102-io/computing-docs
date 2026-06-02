---
description: >-
  How the app generates an Ethereum keypair for a beneficiary who doesn't
  already have one, entirely in the browser — and why we never see the
  private key.
---

# New Account Generation for Beneficiaries

Not every intended beneficiary has an Ethereum address. For a will-like flow to be useful to everyday humans, the app needs a way to produce a fresh Ethereum account for a beneficiary without degrading the security model. 10102 does this with **client-side keypair generation** — the private key is created in the owner's browser, printed onto the [Legacy Claim Card](../user-guide/legacy/legacy-claim-card.md) as a QR code, and _never_ leaves the owner's device.

<figure><img src="../.gitbook/assets/Screen Shot 2024-12-20 at 3.08.40 PM.png" alt=""><figcaption><p>Optional "Generate a new address" flow during beneficiary configuration. The QR code is downloaded directly to the owner's device.</p></figcaption></figure>

## The mechanics

Ethereum accounts are not registered anywhere — they are simply derived from random entropy. The app uses the standard client-side crypto that every major Ethereum wallet uses:

1. **Random entropy** — the browser's `crypto.getRandomValues()` (a WebCrypto primitive backed by the OS CSPRNG) produces 256 bits of random data. This is the same entropy source MetaMask and hardware wallets rely on for key generation.
2. **Private key** — those 256 bits _are_ the private key, represented as a 64-character hex string.
3. **Public key** — derived from the private key via elliptic-curve multiplication on the `secp256k1` curve. This is a one-way operation: you can get the public key from the private key, never the reverse.
4. **Ethereum address** — the last 20 bytes of the Keccak-256 hash of the public key, checksummed per EIP-55.

The whole derivation takes milliseconds and runs synchronously in the browser.

## Custody model: the owner's responsibility

At no point in this flow does 10102 — the company, the app's backend, the smart contracts — see or touch the private key. It exists only:

- In volatile memory in the owner's browser, for the duration of the key-generation step.
- Printed as a QR code on the Legacy Claim Card the owner downloads / prints.
- Wherever the owner physically stores that card afterwards.

The owner is fully responsible for delivering the printed key to the beneficiary through whatever out-of-band channel they trust: sealed envelope with their estate lawyer, safe deposit box, encrypted message, physical handover. The app's job ends when the PDF is generated.

This is the same trust model as a hardware-wallet seed phrase: the chain of custody is physical and human, not digital and automated. That's a feature, not a limitation — it means there's no single digital attack surface for "steal all legacy private keys at once."

## How the beneficiary uses it

At claim time, the beneficiary has two paths:

- **Via the 10102 app** — scan the QR code with the app's import flow, which writes the key into a freshly-generated WalletConnect-compatible session. From there they activate the legacy like any other beneficiary.
- **Via any Ethereum wallet** — scan the QR with MetaMask Import, Rainbow, or a hardware wallet's recovery flow. Then use the legacy's contract address (also on the Legacy Claim Card) via Etherscan's "Write Contract" tab to call the activation function directly. No 10102 UI required.

See [Legacy Claim Card](../user-guide/legacy/legacy-claim-card.md) for the printable artifact and the claim instructions it carries.

## Known limits

- **Printed keys are hot keys.** Once printed, anyone with physical access to the card has the funds. Treat claim cards like cash.
- **Single-key security.** Generated accounts are EOAs with a single key — if the key is lost before claim, the transferred funds are unrecoverable. A beneficiary who is crypto-comfortable should ideally bring their own address instead of using the generated one.
- **No recovery.** There is no recovery service. The app does not store a copy. This is intentional; a custodial copy would be a honeypot.

If any of these feel wrong for a given legacy, the answer is to use a real beneficiary address (e.g. a Safe they control) instead of a generated one. The generated-account path is a convenience for beneficiaries who wouldn't otherwise have an address at all, not a recommendation for high-value plans with crypto-savvy recipients.
