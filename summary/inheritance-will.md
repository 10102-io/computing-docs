# Inheritance will

The **Inheritance** will type use Safe wallet infrastructure, leveraging the battle-tested security of the popular estalished platform. Once activated, an inheritance will provides pre-designated beneficiaries with full wallet permission by adding the beneficiaries as co-signers to a multisig Safe wallet, leveraging the battle-tested security of an established wallet infrastructure.&#x20;

Beneficiaries will become co-owners of the Safe wallet, and can take complete control of the account. They claim the Safe wallet’s staked assets from other platform/protocol (the assets are not stored directly inside the wallet) and other potential non-assets properties, like the reputation, bonus points/rewards, airdrop, etc.

The owner of the will can create, edit or delete a will through both 10102's Digital Inheritance application, as well as through their Safe account. Similarly, beneficiaries can activate a will through both the 10102 app and the Safe platform.

### **Details of the Inheritance will flow** <a href="#x66hygxf4de9" id="x66hygxf4de9"></a>

#### Create a will

* The will owner can create a Safe wallet in [https://app.safe.global](https://app.safe.global/). The will is set as a will contract using Safe Proxy.
* After setting the will contract in Safe Proxy, the will owner can set guard in [https://app.safe.global](https://app.safe.global/) or in the Digital Inheritance's site. The system uses Set Guard Function of SafeGuard.
* The will owner can set module in [https://app.safe.global](https://app.safe.global/) or in the Digital Inheritance's site using Set Module function.
* After setting guard and module, the will owner can configure will details including asset, beneficiaries and time to activation since last outgoing transaction.
* Co-signers of the Safe Wallet will need to sign a transaction to finalize creating the will. Once the minimum number of signatures required in the Safe Wallet is met, the will is created.

#### Edit a will

* After creating the will successfully, the owner can add more beneficiaries and/or adjust the time to activation.
* Co-signers of the Safe Wallet will need to sign a transaction to finalize editing the will. Once the minimum number of signatures required in the Safe Wallet is met, the will is updated.

#### Delete a will

* After creating will successfully, the owner can delete will.
* Co-signers of the Safe Wallet will need to sign a transaction to finalize deleting the will. Once the minimum number of signatures required in the Safe Wallet is met, the will is deleted.

#### **Activate a will**  <a href="#id-5hdkpt7pd7fh" id="id-5hdkpt7pd7fh"></a>

* Any of the designated beneficiaries can make a call to activate a will to the Safe module contract and pay a gas fee.
* SafeModule check the latest timestamps transaction in SafeGuard to see if enough time has passed.&#x20;
* If enough time has passed, Safe Module will execute execTransactionFromModule to SafeProxy ( the will contract)
* The beneficiaries will be added as new co-signers to the Safe Wallet that contains the inheritance. The minimum number of signatures required to initiate a transaction in the Safe wallet remains unchanged.
