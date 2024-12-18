---
description: Technical workflow
---

# Core mechanisms

#### Private Keys

Cryptography is at the core of any decentralized project, and that involves what we call keypairs, or in other words, public and private keys.&#x20;

Private keys are the keys to open safes of this new world, and therefore they need to be well guarded. In our case, private keys are the ones we need to delicately pass around based on one's predefined rules.

#### Smart Contracts

Smart contracts are executers of rules contained in a pre-defined contract. There is a smart contract owner initially, one that can be removed. While often regarded as a flaw, being able to change the rules of a smart contract can come really convenient in real life.&#x20;

For example, the owner of a Safe can decide to change some of the approvers if they have been compromised at any moment, and that is not a bug, but a feature. Similarly, one may want to change the specified heritage in the case of any status change (or relationship).

#### Main Scenario

Person A wants to provide access to the keys of W wallet to person B and person C in the case of passing.&#x20;

A knows that if it didn't connect to W for X months (variable defined by A), something major happened in life, likely reflecting own incapability to access. That's when A wants the computing will to kick in.

A heart-bit mechanism will essentially translate any detected incapability (X months of inactivity) to a trigger. The effective smart contract in place will therefore add B and C as authorized signers.&#x20;

#### Heart-bit

A function defined in the smart contract check for out activities in the owner's wallet on a regular basis, which indicate signs of life. If there is no activity in the specified timeframe, the digital will will be executed.&#x20;

**Notes:**

* Activity can be any of the following:&#x20;
  * Outgoing transaction, claim transaction, or voting transaction.
  * In general, activity here is defined as the owner manually utilizing the safe in any way.
  * The owner is required to sign a message while connecting to the dApp. That way, simply connecting counts as a sign of life in the heart-bit's detection framework.
* Frequency of the heart-bit can be set to daily, weekly, or monthly.

#### Inheritance will

Once activated, an inheritance will provides pre-designated beneficiaries with full wallet permission by adding the beneficiaries as co-signers to a multisig Safe wallet, leveraging the battle-tested security of an established wallet infrastructure.&#x20;

#### Forwarding will type

The forwarding will lets the owner predetermine allocations of specific assets to be sent to each beneficiary's wallet once the will is activated.  A different flow, for a different need. The original owner's wallet from which assets are to be inherited  from can either be a multi-sig wallet such as Safe, or an EOA such as a Ledger, Trezor. or Metamask account.

A combination of any two above can be used in conjunction for more advanced strategies. For example, someone with little kids might want to make sure that until a mature age, a 3rd party can manage some of the funds in case of emergency.&#x20;

#### Beneficiaries, or pre-authorized public key(s)

This part might be one of the most challenging thus far. On one hand, we want to operate via secure and on-chain means only, namely, via Ethereum accounts. On the other hand, the most likely recipients one desires (mainstream) are not familiar enough with the Ethereum rails at this time (nature of any niche emerging technology industry). We discuss here options and ideas to best compromise. Current options from most secure to most friendly:

* An Ethereum public key (address).
* A social wallet address. Another smart contract accessible by guardians, which can be identified by a phone number[^1] providing the corresponding app to such social wallet is installed[^2] (e.g Argent).
* A QR generated printed by the owner. The QR code represents the private key of such conditional pre-authorized public key.
* Printing shall be our main communication channel, as this action is performed by the owner only, avoiding any interaction with a 3rd party service provider.
  * Printing of private keys should come with different options covering the various use cases and level of familiarity with crypto.&#x20;
  * An endpoint to immutable interface should be part of the printed paper for the beneficiary to check at any time, from anywhere. The interface will show the time left to future eligibility, if any. \


[^1]: should we remove this?

[^2]: 
