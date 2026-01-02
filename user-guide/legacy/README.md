# Legacy

### Table of Contents

[Multisig Legacy](./#multisig-legacy)

[Transfer Legacy](./#transfer-legacy)

### Multisig Legacy

The **Multisig Legacy** type use Safe wallet infrastructure, leveraging the battle-tested security of the popular estalished platform. Once activated, an inheritance will provides pre-designated beneficiaries with full wallet permission by adding the beneficiaries as co-signers to a multisig Safe wallet, leveraging the battle-tested security of an established wallet infrastructure.&#x20;

Beneficiaries will become co-owners of the Safe wallet, and can take complete control of the account. They claim the Safe wallet’s staked assets from other platform/protocol (the assets are not stored directly inside the wallet) and other potential non-assets properties, like the reputation, bonus points/rewards, airdrop, etc.

The owner of the legacy can create, edit or delete a contract through both 10102's Digital Inheritance application, as well as through their Safe account. Similarly, beneficiaries can activate a legacy contract through both the 10102 app and the Safe platform.

### Transfer Legacy

The **Transfer Legacy** type will let the owner predetermine allocations of specific assets, which will be sent to each beneficiary's wallet once the contract is activated. This option doesn't require beneficiaries to work together as co-signers as is the case with Multisig Legacy.&#x20;

* The original owner's wallet can either be a multisig wallet such as Safe, or an Ethereum Externally-Owned Account (EOA) such as a Ledger, Trezor, or Metamask account.
* The Transfer Legacy has the same trigger setting using time elapsed since last outgoing transaction as Multisig Legacy.
* Asset allocation: the owner can specify which assets and the % allocation for each listed beneficiary.
* Beneficiaries are added in the form of individual Ethereum addresses. Once activated, the contract will send allocated assets to each beneficiary's Ethereum address.
* The owner can create a legacy contract and approve or deposit assets to the legacy contract (via 10102's Digital Inheritance app).
* At any time before the legacy contract is activated by a beneficiary, the owner can reset the time to activation of the legacy contract by clicking the "I'm still alive" button and pay the gas fee.
* Once the required time to activation has elapsed, the beneficiaries can activate the legacy contract. Activating the contract will incur a gas fee for the beneficiary who initiate the action. The pre-allocated assets will be sent to each beneficiary according to the contract instructions.
* If there are more than 100 transactions (eg. 4+ beneficiaries with 25+ assets for each), the system will send assets in an increment of 100 transactions or less at a time. An option to "Claim remaining fund" will appear until all the allocated assets have been claimed. Anyone of the listed beneficiaries can claim the assets, and the tokens will be sent to all listed beneficiaries.
* Before the legacy contract is activated, the owner can reset its time to activation at any time by clicking the "I'm still alive" button and pay a gas fee.
