# Forwarding will

Specify which assets should be sent to which Ethereum accounts. This option is more specific and straightforward, as it doesn't require co-signers to potentially work together as is the case with inheritance.

* Triggers and duration will be same with Inheritance
* Add beneficiary: Ethereum address&#x20;
* Asset allocation: select available asset to send to each added beneficiary

### Forwarding flow when owner’s wallet is EOA <a href="#sjibdcrqr083" id="sjibdcrqr083"></a>

* The owner create and send assets to the will contract (via 10102's app).
* If the owner dies in real life, then the beneficiaries try to activate the will contract. There will be a notice period, for example 6 months, 1 year, etc..
* During the notice period, if the owner is actually still alive, he can cancel the activation process.
* After the notice period ends, if there’s no required action from the owner, the beneficiaries can claim the assets.

### Requirements when owner’s wallet is smart contract <a href="#id-88n18v90zj9t" id="id-88n18v90zj9t"></a>

* If the owner’s wallet is an AA or multi-sig wallet that has a built-in function to persist the last transaction’s timestamp (or block number) inside their contract storage, the will contract can just call the function to retrieve the owner’s last activity information. (For now this kind of function is not a standard for AA/multisig wallet yet).
* If the owner’s wallet is a Gnosis Safe wallet. We’ll develop a guard contract ([https://docs.safe.global/advanced/smart-account-guards](https://docs.safe.global/advanced/smart-account-guards)) which will persist the timestamp of the owner's last activity. If the owner’s wallet uses this guard, then the wallet can retrieve the necessary information from the guard contract.
* If the owner’s wallet is another type of multi-sig wallet, we need to check whether it’s possible to hook into the outgoing transaction like Gnosis Safe. This depends on the particular multi-sig implementation, and requires more detailed analysis. Currently Gnosis Safe is the market leader for multi-sig wallet so I just use it as the typical use case.

If the owner’s wallet is smart contract but doesn’t satisfy one of above conditions, the flow will be same as EOA’s

### Details of the forwarding flow

#### Create will

* When clicking Create a will -> User select forwarding type -> Show 2 options for user to choose EOA or Safe wallet to be the owner of the will.
* If user select EOA Wallet to become owner of will: -> Navigate to Configure screen to config details include: Will name, List of beneficiaries, Asset allocation details, Activation trigger and Notes.

#### Configure assets allocations

* In asset allocation details, user have to:
  * Approve token for ERC-20
  * Transfer Native tokens to will contract ( Note: All ERC-20 tokens and remaining native tokens in EOA wallet can be still spent normally).
* Asset allocation details for beneficiaries:
  * User can specify the percentage of each beneficiary, as long as the total is 100%.\
    Example: There’re beneficiaries: A, B, C, D. The owner can specify something like: A: 20%, B: 10%, C: 30%, D : 40%. Those percentage will apply for all kind of assets (Ether and tokens, on both EOA and will contract) when transfered to the beneficiaries.
  * Option to specify asset for beneficiaries follow each tokens.

#### Activation trigger

* When will is triggered to activated. Trigger action is public, everyone can trigger
* In our platform, beneficiary is the person who trigger and claim fund.
* Others can come to smart contract without access to our system and trigger the will. -> All ERC-20 tokens left and approved inside EOA and all transferred native token in will contract will be transferred to beneficiaries with the specified percentage before.

#### Edit will

* After creating will successfully, the owner can add more beneficiaries and adjust ther percent, as long as the total is 100%.

#### Delete will

* After creating will successfully, owner can delete will, then all the native token in the will contract will be transferred back to EOA wallet, besides, all approval function of ERC-20 tokens are cancelled.

#### Owner update alive status

* After creating will successfully, The owner have to access to our system mark as he is still alive. ( Click to "I'm still alive" button to call a transaction to SC)

