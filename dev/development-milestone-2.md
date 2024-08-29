---
description: Decentralized indexing, forwarding will type, and more
---

# Development Milestone 2

#### Indexing via TheGraph

There is more immediate work to do following our first milestone, such as using TheGraph for indexing off-chain data such as the list of inherited wills.

#### Forwarding will type

The major part of this milestone however is to fully complete the forwarding will type, which is a complete different flow than the inheritance will type.&#x20;

The forwarding will type essentially locks up specific assets that are designed to be sent to pre-defined beneficiaries in the future. The original owner's wallet does not matter in this case, the instructions however have to be more specific, such as which assets to sent, how much, and where. A different flow, for a different need.

#### Quick summary on the two different flows

* Inheritance = adding signers based on pre-defined schema. (required signatures).
* Forwarding = sending assets based on pre-defined percentages.

#### Web3 Connect options

Verify that at minimum, the following types of connections are supported:\
Metamask, Wallet Connect, Ledger, Trezor, Rainbow, and Coinbase Wallet.

Eventually, all the wallets that are supported on Safe should be supported on 10102's app.&#x20;

#### ENS all around compatibility

Make sure the Ethereum addresses are displayed by their assigned ENS if applicable, in all areas of the dApp.

#### Network switch capability

Allow the selection of main or test networks (e.g Sepolia) within the dApp, so that users can test the concept before connecting their real wallets. Check the Safe app for inspiration here again.&#x20;
