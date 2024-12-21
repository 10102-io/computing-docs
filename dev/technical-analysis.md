# Technical analysis

### Background <a href="#isoyr7fy03sd" id="isoyr7fy03sd"></a>

The purpose of this project is to create a smart contract to hold the assets and will terms. After the owner’s death is confirmed, then the beneficiaries can claim the assets. All of the activities must be done on-chain.

### Expected flow <a href="#eylmkxtodk0g" id="eylmkxtodk0g"></a>

The expected flow is:

* The owner create and send assets to the will contract
* If the owner's wallet doesn’t have any activity during a certain period (for example 6 months or a year, etc..) → the system will determine that the owner has died, and the will is now activated.
* After the will is activated, the beneficiaries can claim the assets.
* Otherwise, the beneficiaries can do nothing with the will.

### Technical limitations <a href="#mga0y489h5al" id="mga0y489h5al"></a>

Due to EVM and Solidity’s current design, it’s impossible to retrieve the information about the last outgoing activity of an arbitrary wallet from a smart contract. This means if the owner’s wallet doesn’t have some special implementation, the will contract has **no way to detect the last outgoing transaction** of that wallet.

In order to make it possible, there are some approaches:

1. Get the owner’s last activity information from an off-chain channel (not preferred since our app is decentralized).
2. Get the owner’s last activity information from an Oracle (technically possible, but currently there’s no decent Oracle that provides this kind of data yet).
3. The owner’s wallet must have some customized functionality.
4. Or we have to change the expected flow.

Will be elaborated in the next section.

Since we don’t prefer to use off-chain methods, decent Oracle is not suitable, and some hybrid methods like ChainLink Function are over complex and still depend on API provider; the most suitable approaches are (3) and (4).

### Proposed new flow <a href="#h36nr8ske5m" id="h36nr8ske5m"></a>

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

### Consideration <a href="#f5k90391kdad" id="f5k90391kdad"></a>

I personally think we should use the same flow for all cases of the owner's wallet, in order to deliver the unified UX to end users. It may be difficult for the users to figure out why sometimes they have to follow this flow, sometimes the flow is different.

There are some other kind of flows I see the similar products have implemented:

* Requires the owner to perform check-in to the will contract regularly. After 6 months if the owner haven’t interacted with the will contract, the system will determine the owner is dead 😅.
* Define the lawyer’s wallet in the will contract, who can determine and execute the will (similar in the real world) - but this will be a completely different business requirement.

The final decision is up to the product owner, but if I were the product owner, and in the context of the current project, I will use the proposed one in section “Owner’s wallet is EOA”, applied for all cases.



### Future release: Forwarding will with Account Abstraction <a href="#id-88n18v90zj9t" id="id-88n18v90zj9t"></a>

* If the owner’s wallet is an AA or multi-sig wallet that has a built-in function to keep track of the last transaction’s timestamp (or block number) inside their contract storage, the will contract can just call the function to retrieve the owner’s last activity information. (For now this kind of function is not a standard for AA/multisig wallet yet).
* If the owner’s wallet is a Gnosis Safe wallet. We’ll develop a guard contract ([https://docs.safe.global/advanced/smart-account-guards](https://docs.safe.global/advanced/smart-account-guards)) which will keep track of the timestamp of the owner's last activity. If the owner’s wallet uses this guard, then the wallet can retrieve the necessary information from the guard contract.
* If the owner’s wallet is another type of multi-sig wallet, we need to check whether it’s possible to hook into the outgoing transaction like Gnosis Safe. This depends on the particular multi-sig implementation, and requires more detailed analysis. Currently Gnosis Safe is the market leader for multi-sig wallet so I just use it as the typical use case.

If the owner’s wallet is smart contract but doesn’t satisfy one of above conditions, the flow will be same as EOA’s
