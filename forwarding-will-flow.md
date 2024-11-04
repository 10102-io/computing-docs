# Forwarding will flow

Specify which assets should be sent to which Ethereum accounts. This option is more specific and straightforward, as it doesn't require co-signers to potentially work together as is the case with inheritance.

* Triggers and duration will be same with Inheritance
* Add beneficiary: Ethereum address&#x20;
* Asset allocation: select available asset to send to each added beneficiary
* If there’s anything left, suggest Inheritance flow

### Owner’s wallet is EOA <a href="#sjibdcrqr083" id="sjibdcrqr083"></a>

If the owner’s wallet is EOA, the possible flow will be like:

* The owner create and send assets to the will contract
* If the owner dies in real life, then the beneficiaries try to activate the will contract. There will be a notice period, for example 6 months, 1 year, etc..
* During the notice period, if the owner is actually still alive, he can cancel the activation process.
* After the notice period ends, if there’s no action from owner, the beneficiaries can claim the assets

Compared to the original expected flow, the beneficiaries may have to wait longer until being able to claim assets. But our target is to keep the system decentralized though.

### Owner’s wallet is smart contract <a href="#id-88n18v90zj9t" id="id-88n18v90zj9t"></a>

If the owner’s wallet is a smart contract. There are some possible cases where they have customized functionality so we can keep the original expected flow.

* If the owner’s wallet is an AA or multi-sig wallet that has a built-in function to persist the last transaction’s timestamp (or block number) inside their contract storage. In this case the will contract can just call the function to retrieve the owner’s last activity information. (For now this kind of function is not a standard for AA/multisig wallet yet).
* If the owner’s wallet is a Gnosis Safe wallet. We’ll develop a guard contract ([https://docs.safe.global/advanced/smart-account-guards](https://docs.safe.global/advanced/smart-account-guards)) which will persist the timestamp of the owner's last activity. If the owner’s wallet uses this guard, then the wallet can retrieve the necessary information from the guard contract.
* If the owner’s wallet is another type of multi-sig wallet, we need to check whether it’s possible to hook into the outgoing transaction like Gnosis Safe. This depends on the particular multi-sig implementation, and requires more detailed analysis. Currently Gnosis Safe is the market leader for multi-sig wallet so I just use it as the typical use case.

If the owner’s wallet is smart contract but doesn’t satisfy one of above conditions, the flow will be same as EOA’s
