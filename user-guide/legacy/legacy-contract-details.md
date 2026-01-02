# Legacy Contract Details

### **Table of contents** <a href="#cy3oh4wk17z3" id="cy3oh4wk17z3"></a>

[My Legacy](legacy-contract-details.md#l6ugmfkjrqu0)

[My Legacy Contract Details Screen (with Safe Wallet)](legacy-contract-details.md#my-wills-details-screen-with-safe-wallet)

[My Legacy Contract Details Screen (with EOA)](legacy-contract-details.md#my-legacy-contract-details-screen-with-eoa)

[My Inherited Legacy](legacy-contract-details.md#id-9tlp3kbavosd)

[My Inherited Legacy Details Screen](legacy-contract-details.md#my-inherited-legacy-contract-details-screen)

### **My Legacy** <a href="#l6ugmfkjrqu0" id="l6ugmfkjrqu0"></a>

<figure><img src="../../.gitbook/assets/screencapture-app-10102-io-2026-01-01-18_05_42.png" alt=""><figcaption></figcaption></figure>

* **My legacy** displays all the legacy contracts user has created, or legacy contracts created with a Safe Wallet that the user is a co-signer.
* User can filter or search contracts with search bar and ratio.
* User can click to right arrow of each contract to view the details

### **My Legacy Contract Details Screen (with Safe Wallet)**

After a legacy contract is created with a Safe Wallet, it will initially have the status **Needs finalizing**, which requires a minimum number of co-signers to sign on the transaction in order to finalize creating the legacy contract. The minimum number of signatures is set in the Safe Account. Co-signers can provide signatures through the 10102's Digital Inheritance app, or through the Safe wallet platform.

<figure><img src="../../.gitbook/assets/Screen Shot 2026-01-01 at 6.18.20 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screen Shot 2026-01-01 at 6.20.30 PM.png" alt=""><figcaption></figcaption></figure>

* Co-signers of the Safe account can finalize the legacy contract, which will incur a gas fee. The will status will then change to **Live**. Similarly, user can execute this transaction in Safe Wallet platform independent of 10102's UI.
* Once the minimum number of signatures required in the Safe wallet is met, the legacy contract is then **Live**. Once the contract status is **Live,** user can **edit/delete** the contract by clicking button **Edit contract/ Delete contract.** Check out the [Edit or delete a legacy contract](edit-or-delete-a-legacy-contract.md) guide for more details

<figure><img src="../../.gitbook/assets/Screen Shot 2026-01-01 at 6.29.12 PM.png" alt=""><figcaption></figcaption></figure>

### **My Legacy Contract Details Screen (with EOA)**

After creating, the legacy contract status is immediately **Live.** Unlike wills created with a Safe wallet, no finalizing is needed.

<figure><img src="../../.gitbook/assets/screencapture-app-10102-io-detail-legacy-0x16TransferEOA-2026-01-01-18_43_28.png" alt=""><figcaption></figcaption></figure>

* At anytime before the legacy contract is activated, the contract owner can click the button **“I’m still alive”** to reset time to activation. This action will incur a gas fee.
* When the legacy contract is live, the contract owner can **edit/delete** the contract by clicking the button **Edit contract/ Delete contract.** Check out the [Edit or delete a legacy contract](edit-or-delete-a-legacy-contract.md) guide for more details.

### **My Inherited Legacy** <a href="#id-9tlp3kbavosd" id="id-9tlp3kbavosd"></a>

* User can view the list of Inherited legacy contracts which list them as a beneficiary
* Clicking on the right arrow in right side of the contract card will take the user to the legacy contract details screen

<figure><img src="../../.gitbook/assets/screencapture-app-10102-io-2026-01-01-18_44_45.png" alt=""><figcaption></figcaption></figure>

### **My Inherited Legacy Contract Details Screen**

<figure><img src="../../.gitbook/assets/screencapture-app-10102-io-detail-legacy-0x16TransferEOA-2026-01-01-18_47_58.png" alt=""><figcaption></figcaption></figure>

The status **Not activated** indicates that no beneficiaries have activated the legacy to claim fund, or the legacy contract time to activation has not fully elapsed. Once enough time has passed, and one of the beneficiaries successfully initiates the claiming fund process, the contract status will become **Activated**. More on [Activate a legacy contract and claim funds](activate-a-legacy-contract-and-claim-funds.md).&#x20;
