# Transfer Legacy

The **Transfer Legacy** type will let the owner predetermine allocations of specific assets, which will be sent to each beneficiary's wallet once the contract is activated. This option doesn't require beneficiaries to work together as co-signers as is the case with Multisig Legacy. The original owner's wallet from which assets are to be inherited  from can either be a multi-sig wallet such as Safe, or an Ethereum Externally-Owned Account (EOA) such as a Ledger, Trezor, or Metamask account.

* The Transfer Legacy has the same trigger setting using time elapsed since last outgoing transaction as Multisig Legacy.
* Asset allocation: the owner can specify which assets and the % allocation for each listed beneficiary.
* Beneficiaries are added in the form of individual Ethereum addresses. Once activated, the contract will send allocated assets to each beneficiary's Ethereum address.

### Transfer Legacy with an Externally-Owned Account (EOA) Wallet <a href="#sjibdcrqr083" id="sjibdcrqr083"></a>

* The owner can create a legacy contract and approve or deposit assets to the legacy contract (via 10102's Digital Inheritance app).
* At any time before the legacy contract is activated by a beneficiary, the owner can reset the time to activation of the legacy contract by clicking the "I'm still alive" button and pay the gas fee.
* Once the required time to activation has elapsed, the beneficiaries can activate the legacy contract. Activating the will will incur a gas fee for the beneficiary who initiate the action. The pre-allocated assets will be sent to each beneficiary according to the will's instructions.
* If there are more than 100 transactions (eg. 4+ beneficiaries with 25+ assets for each), the system will send assets in an increment of 100 transactions or less at a time. An option to "Claim remaining fund" will appear until all the allocated assets have been claimed. Anyone of the listed beneficiaries can claim the assets, and the tokens will be sent to all listed beneficiaries.

### Flow of the Transfer Legacy

#### Create a transfer legacy contract

* There are 2 options for user to choose from, creating a Transfer Legacy contract with an EOA or a Safe wallet.
* Once the user selects an option, the user will be able to set up details include: Legacy contract name, List of beneficiaries, Asset allocation details, Activation trigger and Notes.

#### Configure assets allocations

* In order to ensure that beneficiaries can receive the assets, the owner will have to:
  * Approve ERC-20 tokens in order for the contract to have the permission to transfer assets to beneficiaries' wallets when the will is activated. All ERC-20 tokens in the EOA wallet can still be spent normally.
  * Transfer Native tokens (ETH) to the legacy contract. Owner can deposit only a portion of their ETH. The contract will hold custody of the deposited ETH. The owner can always edit the legacy contract and withdraw the deposited ETH from the will contract.&#x20;
* Asset allocation details for beneficiaries:
  * User can specify the percentage of each beneficiary, as long as the total is 100%.\
    Example: There are four beneficiaries: A, B, C, D. The owner can specify that A will receive 20% of the assets, B 10%, C 30%, and D 40%. The allocation will apply to the deposited Ether and approved ERC-20 tokens from the EOA wallet.

#### Edit a transfer legacy contract

* After creating the legacy contract successfully, the owner can add more beneficiaries and adjust the allocation, as long as the total is 100%.
* The owner can also approve more tokens, deposit or withdraw ETH from the loegacy contract.
* Approving, depositing, withdrawing tokens, and editing Note to beneficiaries will not reset the contract's time to activation. All other actions will reset the contract's time to activation.

#### Delete a transfer legacy contract

* After creating a legacy contract, the owner can delete it; then the native token(s) in the contract will be transferred back to owner's EOA wallet. All approval functions of ERC-20 tokens are cancelled.

#### Activate a transfer legacy contract

* Activating a legacy contract is a public action.
* On our 10102's Digital Inheritance app, one of the beneficiaries can activate a legacy contract and claim the funds for all beneficiaries.
* Others can come to the smart contract available on the Ethereum blockchain without access to our system to activate the legacy contract. All approved ERC-20 tokens in the EOA wallet and deposited native token(s) in the legacy contract will be transferred to the listed beneficiaries based on the predetermined allocations.

#### Reset time to activation

* Before the legacy contract is activated, the owner can reset its time to activation at any time by clicking the "I'm still alive" button and pay a gas fee.

