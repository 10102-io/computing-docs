# Forwarding will

The **Forwarding** will lets the owner predetermine allocations of specific assets, which will be sent to each beneficiary's wallet once the will is activated. This option doesn't require beneficiaries to work together as co-signers as is the case with **Inheritance**. The original owner's wallet from which assets are to be inherited  from can either be a multi-sig wallet such as Safe, or an Ethereum Externally-Owned Account (EOA) such as a Ledger, Trezor. or Metamask account.

* The Forwarding will have the same trigger setting using time elapsed since last outgoing transaction as Inheritance.
* Asset allocation: the owner can specify which assets and the % allocation for each listed beneficiary.
* Beneficiaries are added in the form of individual Ethereum addresses. Once activated, the will will send allocated assets to each beneficiary's Ethereum address.

### Forwarding will with an Externally-Owned Account (EOA) Wallet <a href="#sjibdcrqr083" id="sjibdcrqr083"></a>

* The owner can create a will contract and approve or deposit assets to the will contract (via 10102's Digital Inheritance app).
* At any time before the will is activated by a beneficiary, the owner can reset the time to activation of the will contract by clicking the "I'm still alive" button and pay the gas fee.
* Once the required time to activation has elapsed, the beneficiaries can activate the will contract. Activating the will will incur a gas fee for the beneficiary who initiate the action. The pre-allocated assets will be sent to each beneficiary according to the will's instructions.
* If there are more than 100 transactions (eg. 4+ beneficiaries with 25+ assets for each), the system will send assets in an increment of 100 transactions or less at a time. An option to "Claim remaining fund" will appear until all the allocated assets have been claimed. Anyone of the listed beneficiaries can claim the assets, and the tokens will be sent to all listed beneficiaries.

### Details of the Forwarding flow

#### Create a will

* There are 2 options for user to choose from, creating a Forwarding will with an EOA or a Safe wallet.
* Once the user selects an option, the user will be able to set up details include: Will name, List of beneficiaries, Asset allocation details, Activation trigger and Notes.

#### Configure assets allocations

* In order to ensure that beneficiaries can receive the assets, the owner will have to:
  * Approve ERC-20 tokens in order for the contract to have the permission to transfer assets to benefiaciaries' wallets when the will is activated. All ERC-20 tokens in the EOA wallet can still be spent normally.
  * Transfer Native tokens (ETH) to the will contract. Owner can deposit only a portion of their ETH. The contract will hold custody of the deposited ETH. The owner can always edit the will and withdraw the deposited ETH from the will contract.&#x20;
* Asset allocation details for beneficiaries:
  * User can specify the percentage of each beneficiary, as long as the total is 100%.\
    Example: There are four beneficiaries: A, B, C, D. The owner can specify that A will receive 20% of the assets, B 10%, C 30%, and D 40%. The allocation will apply to the deposited Ether and approved ERC-20 tokens from the EOA wallet.

#### Edit a will

* After creating the will successfully, the owner can add more beneficiaries and adjust the allocation, as long as the total is 100%.
* The owner can also approve more tokens, deposit or withdraw ETH from the will contract.
* Approving, depositing, withdrawing tokens, and editing Note to beneficiaries will not reset the will contract's time to activation. All other actions will reset the will's time to activation.

#### Delete a will

* After creating a will, the owner can delete will, then all the native token in the will contract will be transferred back to owner's EOA wallet. All approval functions of ERC-20 tokens are cancelled.

#### Activate a will

* Activating a will is a public action.
* On our 10102's Digital Inheritance app, one of the beneficiaries can activate a will and claim the fund for all beneficiaries.
* Others can come to the smart contract available on the Ethereum blockchain without access to our system to activate the will. All approved ERC-20 tokens in the EOA wallet and all deposited native token in the will contract will be transferred to the listed beneficiaries based on the predetermined allocations.

#### Reset time to activation

* Before the will is activated, the owner can reset the will's time to activation at any time by clicking the "I'm still alive" button and pay a gas fee.

