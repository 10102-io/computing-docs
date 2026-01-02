# Activate a Legacy Contract and Claim Funds

### **Table of Contents** <a href="#cy3oh4wk17z3" id="cy3oh4wk17z3"></a>

[Check contract's status & claim funds](activate-a-legacy-contract-and-claim-funds.md#check-contracts-status-and-claim-funds)

[Once the contract is activated](activate-a-legacy-contract-and-claim-funds.md#once-the-contract-is-activate)

[Claim funds using an explorer](activate-a-legacy-contract-and-claim-funds.md#claiming-funds-using-an-explorer)

If the legacy contract is created with a Safe wallet, the beneficiary can activate using either 10102 app or the Safe module contract.

If the legacy contract is created with an EOA, the beneficiary can activate using either 10102 app or a blockchain explorer, such as Etherscan.

### Check Contract's Status & Claim Funds

Beneficiary can click **Check contract's status & claim funds**, which will prompt the system to check if enough time has passed for the contract to be activated.

<figure><img src="../../.gitbook/assets/Screen Shot 2026-01-01 at 7.44.51 PM.png" alt=""><figcaption></figcaption></figure>

* If the required time has passed, the beneficiary will be eligible to claim funds.The contract will execute asset distribution logic to designated beneficiaries. The status of the will will become **Activated**.
* If not enough time has passed, the system will prompt an error. The legacy contract remains not activated.
* Any of the designated beneficiaries can make a call to activate a legacy contract. Distribution will happen to all designated beneficiaries. The beneficiary that activates the contract will pay a gas fee.

### Once the Contract is Activated

* For Multisig legacy contracts, the beneficiaries will be added as new co-signers to the Safe Wallet that contains the inheritance. The minimum number of signatures required to initiate a transaction in the Safe wallet is the minimum number of required signatures specified in the contract by the owner.
* For Transfer legacy contracts, all approved tokens in the EOA wallet and deposited native token(s) in the legacy contract will be transferred to the listed beneficiaries based on the predetermined allocations. The system can handle a maximum of 100 transactions at a time. If there are more than 100 transactions needed in the asset distribution process (eg. there are 4 beneficiaries and 25+ assets), the system will distribute assets in multiple batches. The button **Claim Remaining Fund** will be available if there are more assets to be claimed in the will contract.<br>

<figure><img src="../../.gitbook/assets/Screen Shot 2026-01-01 at 7.47.28 PM.png" alt=""><figcaption></figcaption></figure>

* Once all assets are distributed, the system will show that asset distribution is complete.&#x20;

### Claim Funds Using an Explorer

In the current design, transfer legacy contract can be activated directly on the explorer (such as Etherscan) through the Router contract, which requires the **Legacy ID and the asset token addresses as inputs.**

<figure><img src="../../.gitbook/assets/6325572570664602402 (1).jpg" alt=""><figcaption></figcaption></figure>

* Legacy ID is included in the email notifications sent to beneficiaries when the contract is ready to activate.&#x20;
* The list of asset token addresses can be looked up via the legacy contract address, which is also provided in the email.\
  \
  (When beneficiaries activate the legacy through the UI, the asset list is automatically fetched from the subgraph)
