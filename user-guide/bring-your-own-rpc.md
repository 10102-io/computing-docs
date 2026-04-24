---
description: >-
  Route the app's Ethereum reads through your own RPC endpoint instead
  of the default providers. Useful when the defaults are throttled,
  when you want privacy from third-party node operators, or when you
  simply prefer your own infrastructure.
---

# Bring your own RPC endpoint

Every action in 10102 Computing Legacy that reads on-chain state — checking a legacy's status, listing beneficiaries, verifying ownership, etc. — goes through an Ethereum JSON-RPC endpoint. By default the app rotates through a list of providers we configure, so a single provider being slow or rate-limited doesn't take the app down.

If you have your own node, a paid Alchemy/Infura/drpc key, or simply prefer not to depend on the providers we ship, you can point the app at your endpoint instead.

## How it works

The app supports the draft [dAppSpec](https://ethereum-magicians.org/t/new-erc-best-practices-for-dapps-dappspec/24407) query-parameter convention:

```
?ds-rpc-<chainId>=<url-encoded-rpc-url>
```

When present, the endpoint you supply is used for that chain **with higher priority than anything we configure**. Any provider throttling on our side is bypassed entirely.

The override is also remembered across reloads (saved to your browser's local storage for this origin), so you only have to paste the URL once.

## Examples

### Mainnet

Append to any page URL:

```
?ds-rpc-1=https%3A%2F%2Feth-mainnet.g.alchemy.com%2Fv2%2FYOUR_KEY
```

or, with a self-hosted node:

```
?ds-rpc-1=https%3A%2F%2Frpc.mynode.example%2F
```

### Sepolia (testnet)

```
?ds-rpc-11155111=https%3A%2F%2Fsepolia.infura.io%2Fv3%2FYOUR_KEY
```

### Multiple chains at once

You can override both chains in the same URL:

```
?ds-rpc-1=https%3A%2F%2Fmainnet.example%2F&ds-rpc-11155111=https%3A%2F%2Fsepolia.example%2F
```

## Clearing a saved override

Either:

- Navigate to any page with an **empty** override value, which resets the saved endpoint:

  ```
  ?ds-rpc-1=
  ```

- Or clear your browser's site data for the app domain. Both remove the persisted override; the app will fall back to the default providers on the next load.

## Verifying the override is active

Open the browser's developer console (usually **F12** → Console tab). When an override is in effect, you'll see a line like:

```
[rpc] chain 1: using user-override from ?ds-rpc-1= (stored for this origin)
```

After reloads (without the query parameter), the same log appears but mentions it's using the persisted value:

```
[rpc] chain 1: using persisted user-override (clear with ?ds-rpc-1=)
```

## Things to know

{% hint style="info" %}
**What this changes and what it doesn't.** The override affects **read** calls the app makes to inspect chain state. It does **not** change how your wallet connects to the network or where your wallet signs/submits transactions — that is controlled entirely by your wallet's own RPC settings. If you want your transactions to also go through your own node, set that inside your wallet (e.g. MetaMask → Settings → Networks).
{% endhint %}

{% hint style="warning" %}
**Trust the endpoint you point at.** An RPC provider can, in principle, serve inaccurate reads or link requests to your IP address. Use endpoints operated by parties you're comfortable relying on. The app only accepts `http://` and `https://` URLs; other schemes (like `javascript:` or `data:`) are rejected so the override can't be used to run code.
{% endhint %}

{% hint style="info" %}
**The override is per-browser, per-origin.** Clearing your browser data, switching browsers, or using a private-browsing window all reset the persisted override to the defaults.
{% endhint %}

## See also

- [Authentication](./authentication.md) — how wallets connect and submit transactions.
- [Concepts](./concepts.md) — what the app does with chain state it reads.
