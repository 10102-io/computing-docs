# Multisig Legacy

The **Multisig Legacy** type use Safe wallet infrastructure, leveraging the battle-tested security of the popular estalished platform. Once activated, an inheritance will provides pre-designated beneficiaries with full wallet permission by adding the beneficiaries as co-signers to a multisig Safe wallet, leveraging the battle-tested security of an established wallet infrastructure.&#x20;

Beneficiaries will become co-owners of the Safe wallet, and can take complete control of the account. They claim the Safe wallet’s staked assets from other platform/protocol (the assets are not stored directly inside the wallet) and other potential non-assets properties, like the reputation, bonus points/rewards, airdrop, etc.

The owner of the legacy can create, edit or delete a contract through both 10102's Digital Inheritance application, as well as through their Safe account. Similarly, beneficiaries can activate a legacy contract through both the 10102 app and the Safe platform.

### **Flow of the Multisig Legacy** <a href="#x66hygxf4de9" id="x66hygxf4de9"></a>

#### Create a multisig legacy contract

* The legacy owner can create a Safe wallet in [https://app.safe.global](https://app.safe.global/). The legacy is set as a legacy contract using Safe Proxy.
* After setting the legacy contract in Safe Proxy, the will owner can set guard in [https://app.safe.global](https://app.safe.global/) or in the Digital Inheritance's site. The system uses Set Guard Function of SafeGuard.
* The legacy owner can set module in [https://app.safe.global](https://app.safe.global/) or in the Digital Inheritance's site using Set Module function.
* After setting guard and module, the legacy owner can configure will details including asset, beneficiaries and time to activation since last outgoing transaction.
* Co-signers of the Safe Wallet will need to sign a transaction to finalize creating the legacy contract. Once the minimum number of signatures required in the Safe Wallet is met, the legacy contract is created.

#### Edit a multisig legacy contract

* After creating the legacy contract successfully, the owner can add more beneficiaries and/or adjust the time to activation.
* Co-signers of the Safe Wallet will need to sign a transaction to finalize editing the legacy contract. Once the minimum number of signatures required in the Safe Wallet is met, the contract is updated.

#### Delete a multisig legacy contract

* After creating legacy contract successfully, the owner can delete will.
* Co-signers of the Safe Wallet will need to sign a transaction to finalize deleting the legacy contract. Once the minimum number of signatures required in the Safe Wallet is met, the legacy contract is deleted.

#### **Activate a** multisig legacy contract  <a href="#id-5hdkpt7pd7fh" id="id-5hdkpt7pd7fh"></a>

* Any of the designated beneficiaries can make a call to activate a legacy contract to the Safe module contract and pay a gas fee.
* SafeModule check the latest timestamps transaction in SafeGuard to see if enough time has passed.&#x20;
* If enough time has passed, Safe Module will execute execTransactionFromModule to SafeProxy ( the legacy contract)
* The beneficiaries will be added as new co-signers to the Safe Wallet that contains the inheritance. The minimum number of signatures required to initiate a transaction in the Safe wallet remains unchanged.
