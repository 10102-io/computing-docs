---
description: Technical workflow
hidden: true
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



