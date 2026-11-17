---
description: >-
  Two switches in the header. Hide amounts takes your balances off the
  screen for when someone else can see it. Cypherpunk Mode keeps the app to
  legacies and timelocks, with far fewer services behind it.
---

# Hide amounts and Cypherpunk Mode

On a computer both switches sit in the header. On a phone the eye is in the top bar and the Cypherpunk switch is at the bottom of the menu. The eye shows once there is an account (see below). Neither changes anything on the blockchain or in your contracts: they change what this browser shows and whom it talks to.

## Hide amounts

The eye button hides what your account holds, for when you share your screen or sit in front of a big one. On a computer it is in the header, left of the Cypherpunk switch; on a phone it is in the top bar, beside the menu button.

**What it hides.** Your balances, your totals, and the amounts in your lists and detail pages, in tokens and in dollars alike, with any tooltip that repeats them. A hidden amount is not on the page at all: the same four dots stand in for every value, so neither the text, nor its width, nor a copy of the page gives a figure away.

**What stays visible.**

- Amount fields and what you typed in them.
- The step where you check or confirm something before your wallet asks, and the result after it. Nobody should sign for a figure they cannot see.
- Our prices and fee rates, shares, dates, waits, counts, addresses and names.
- Printed or downloaded sheets and cards.

Where a hint would give a balance away (a suggested amount, a field that would start at your full balance), it turns neutral while amounts are hidden.

**How it behaves.**

- It acts at once, without a reload. Press the eye again to show amounts.
- The choice is kept on this device, and a second tab follows it. It does not travel to your other devices.
- The eye is there once a wallet is connected or an email account is signed in. Without an account it shows only on a page that has a hidden figure on it, a gift's amount on its claim page for example, so a hidden figure always has its way back.
- If you disconnect with amounts hidden, the next account on this device opens with its amounts hidden too.

{% hint style="info" %}
Hide amounts is for the people who can see your screen. It does not hide anything on the blockchain: your balances remain public there, as they always were.
{% endhint %}

## Cypherpunk Mode

Cypherpunk Mode is the app with fewer third-party services and plain on-chain data. In the mode the app talks to your wallet, the blockchain (your RPC) and the contract indexer, and to our own server only in the cases listed under Honest limits below: no analytics, no error reporting, no AI assistant, no email features.

**Turning it on and off.** Press **Cypherpunk** in the header (on a phone, at the bottom of the menu). The app reloads into the mode; press it again to leave, and it reloads again. The header switch is the lasting choice for this browser. A link ending in `?cypherpunk=1` turns the mode on for that tab only, and closing the tab brings your own choice back.

**What it keeps.** Legacies and timelocks, the two foundations. The menu reads Legacy and Timelock, and the app opens on your legacies instead of Home. Creating, editing, checking in, claiming and withdrawing all work as they do outside the mode.

**What it leaves out.**

- Home with Your setup, Quick Actions, Guardian, What's new and the Pricing page.
- In Settings: email reminders, wallet alerts and Devices (sign-in on each device). They store an email or a name on our server, so the mode does not offer them.
- The products built on legacies and timelocks that need our server: the Shield Vault, Stake & Shield, Save, Save on autopilot and the email wallet.

**Product pages opened by their address.** If you open one of those five pages in the mode, by a bookmark or a link, you get a short notice instead of the page: "(product) is not part of Cypherpunk Mode". Nothing of the product is loaded and nothing is asked of our server while it shows. **Leave Cypherpunk Mode** stores the mode off and reloads on the same page; **Back to Home** stays in the mode.

**What is never behind the notice.** Pages for acting on something that already exists stay open in every mode: a gift's claim page, a Shield Vault holding's status page and its printed-card claim, the Save on autopilot stop link, and the heir and executor pages of a legacy. The Shield Vault page is also where a recovery sheet stops a withdrawal, so its notice carries a **Stop it** button, and `app.10102.io/shield#stop` opens the stop form directly in the mode too.

**Sending a gift in the mode.** Heirloom stays open, because a sealed gift is a timelock, and the mode's Timelock page links to it. In the mode, sending a gift asks nothing of our servers:

- A gift goes to a wallet address or onto a printed card. "Just their email" is not offered.
- After sealing, the app does not offer to email the claim link. The link, its QR code and the printable card remain.
- Claiming is unchanged.

Outside the mode, the Heirloom page is as [Heirloom](./heirloom.md) describes it.

{% hint style="info" %}
**Honest limits.** The mode still reads the chain through an RPC provider and the contract indexer, and both can see your address and your IP address. To choose the RPC yourself, see [Bring your own RPC](./bring-your-own-rpc.md). A Safe legacy also uses Safe's transaction service, and a WalletConnect wallet uses WalletConnect's relay. Our own server is still asked in these cases, in the mode as outside it:

- Opening the page of a wallet legacy or of a timelock asks our relay whether a free send is available. The request carries no address; our server sees your IP address.
- A claim without ETH, a Premium check-in and a Premium withdrawal of your own timelock that are sent for free go through our relay, with your signature. Sending the transaction from your own wallet does not.
- If you opened the app through a link that carries a label (`?src=`, `?utm_source=`, `?from=`), creating a wallet legacy or a timelock on Ethereum reports that transaction's hash and the label to our server. Open the app without such a link to avoid it. A Premium purchase is reported the same way, also with a partner code (`?via=`).
- The pages that stay open in the mode for acting on something that exists (a gift's claim page, the Shield Vault status, claim and stop, the Save on autopilot stop link) talk to our server as they do outside it.
{% endhint %}
