# Contracts created with Safe SDK

A [Safe wallet](https://app.safe.global/) is a multi-signature smart account management tool on Ethereum that allows users to manage their crypto assets. A predefined number of private keys is required to sign off on any transaction, so no single person can execute a transaction without the consent of others.&#x20;

[Safe SDK ](https://docs.safe.global/sdk/overview)is a software development kit that enables developers to integrate the Gnosis Safe's secure, multi-signature wallet functionality into their own applications.

**10102's Digital Inheritance** uses the following smart contracts for wills created using a Safe wallet:

{% embed url="https://docs.safe.global/advanced/smart-account-guards" %}
10102's Digital Inheritance uses Safe Guard to keep track of the timestamp of the owner's last activity.&#x20;
{% endembed %}

{% embed url="https://docs.safe.global/advanced/smart-account-modules" %}
10102's Digital Inheritance uses Safe Modules to add custom logic of the will to the Safe wallet.&#x20;
{% endembed %}

### Table of contents

[Create a new Safe Wallet](contracts-created-with-safe-sdk.md#create-a-new-safe-wallet)

[Execute a transaction using Safe wallet](contracts-created-with-safe-sdk.md#execute-a-transaction-using-safe-wallet)

[Set Guard and Module in Safe wallet](contracts-created-with-safe-sdk.md#set-guard-and-module-in-safe-wallet)

[Create a will](contracts-created-with-safe-sdk.md#create-a-will)

[Edit a will](contracts-created-with-safe-sdk.md#edit-a-will)

[Delete a will](contracts-created-with-safe-sdk.md#delete-a-will)

[Activate a will](contracts-created-with-safe-sdk.md#activate-a-will)

### Create a new Safe Wallet:

* Navigate to Safe account at:[ https://app.safe.global/new-safe/create](https://app.safe.global/new-safe/create)
* Connect wallet and fill the name of your Safe wallet and the network that you want to use.
* Set the list of signers  and set the minimum signature number of the safe wallet (threshold)
* Sign and execute to create a new Safe account

### Execute a transaction using Safe wallet:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdUAX8fyDMctTLd8DNg1WUg4wgRhf8zTJ3PJqGHfZUlD1GeBCBiTHsYnFNxtTTVgBBgYQQA2zIThhvARxm9TsVVGS-Hfq8COYKpu2SNHAZIgqXotUikGfyMPRsfNANo2ix8m9Mi?key=FU1CpoPnSDCdWQuIWr-ldA" alt=""><figcaption></figcaption></figure>

In order to execute a transaction using a Safe wallet, the co-signers will have to sign the transaction and meet the  minimum number of signatures set by the Safe wallet's owner.

* Navigate to Safe account at: [https://app.safe.global](https://app.safe.global), click on new transaction and navigate to Transaction Builder
* Fill in the address and ABI of the smart contract that you want to execute.
* Sign and execute transaction.  Once the transaction is executed, last timestamp will be updated into the safe guard contract.

### Set Guard and Module in Safe wallet:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcnFS-i_CuXZ4yjW6sUtTDcRUmoHvObtiSU9rTRtD2MX2oUkGAHumBeX_o_uf8mhyp1w0uqJHNpIWlaI1zmqdFkA3hU1wz9JeHB5AAo5OYvaqkn1au0P1jAQD1PhTbBaJgwu9DI?key=FU1CpoPnSDCdWQuIWr-ldA" alt=""><figcaption></figcaption></figure>

* Navigate to Safe account at: [https://app.safe.global](https://app.safe.global). Click on New transaction and navigate to Transaction Builder. Next, fill in your Safe wallet address and use Implementation ABI
* Select method: setGuard and fill the guard address in the parameter. Next, click Add new transaction
* Select method: enableModule and fill the will address in the parameter. Next, click Add new transaction, then sign and execute transaction
* Safe Wallet contract will emit the ChangeGuard event. Subgraph will listen and update guard.
* Safe Wallet contract will emit the EnableModule event. Supgraph will listen and update module.

### Create a will

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcrsD5AmhGp8plNDwV5MMqDZaXzAlB6mpyfJn2injE027oPVLdhhu4XF54ugjt9-4qltU_GmvRT7HNPm6kfuNw5wJUa6Xdu1JJszsmX4VADhA-_SV15HVy3951SSclq1j3fPP4haQ?key=FU1CpoPnSDCdWQuIWr-ldA" alt=""><figcaption></figcaption></figure>

* Once a user create a will, Safe Guard and Safe Module (will contract) are created.
* Safe Guard and the Safe Module is attached to the owner's Safe wallet. Last activity's timestamp of the will owner is initialized into Safe Gguard.
* The Router contract will emit WillCreated event. Subgraph will then listen and create a new WillDetail entity.

### Edit a will

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdEf0N2CnjowV1xYWxfrWX5f-faBPeG2mE9LMj1wr9F9sIPSUHI1RTq3ViGrtXB__tPRqeNECW-lQdPcO7kknJB5fVLM5oHUtJGzUiQLFOaFF0AsqQFvg61QyMdqam4ZIvNgeEg?key=FU1CpoPnSDCdWQuIWr-ldA" alt=""><figcaption></figcaption></figure>

* Once the user edits the will, and after the co-signers of the Safe wallet sign and execute the transaction to edit the will, the will contract is updated in Safe Module (the will contract).
* Router contract emit WillCreated event for Subgraph to listen and update the will with new information.
* The last activity's timestamp is updated in Safe Guard.

### Delete a will

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcaI0QDDoyc6sW_gH5MwffUmU7nLZnN0t8ZBlRCjnhf6bq4JM1xxyemR1lyGm-Ehj0ExTBmUun-yuylaeJOIm4Bbo6c0Y-Cdl1jykCQeSGeP7DVUHBYuKh9F78w2SMuDltFV-c3HA?key=FU1CpoPnSDCdWQuIWr-ldA" alt=""><figcaption></figcaption></figure>

* From 10102's Digital Inheritance frontend, the owner can delete a will. After the co-signers of the Safe wallet sign and execute the transaction to delete the will, Safe Guard and Safe Module (will contract) is removed from the owner's Safe wallet.
* The owner can also delete a will through their Safe account at [https://app.safe.global](https://app.safe.global). Click on New transaction and navigate to Transaction Builder. Next, fill in the Safe wallet address and use Implementation ABI.
* Select method as setGuard and fill in the guard address: 0x0 into parameter. Next, click on Add new transaction.
* Select method: DisableModule and fill the will address in the parameter. Next, click on Add new transaction, then sign and execute transaction
* Safe Wallet contract will emit the ChangeGuard event. Subgraph will listen and update guard
* Safe Wallet contract will emit the DisableModule event. Supgraph will listen and update the will status to deleted.

### Activate a will

* When a beneficiary check the will's status, the ActiveWill function checks whether the will can be activated. If not enough time (as specified by the will) has passed since the last outgoing transaction, the will contract can’t be activated.&#x20;
* Once the specified time has elapsed, the will contract can be activated by one of the beneficiaries.

#### Inheritance will

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXerNstTonSm4yhmeWymCeVsr4FBJa6Pzw--YDH_DIQZ04lznTFOBiupTIa9S6TpKOG6WRkHIG3G2OGB9uRF1FCJDLQEZnKl9loBKNs9JgSe4_oQh-oLNcGw4M_r-r5XgJM1zaoYww?key=FU1CpoPnSDCdWQuIWr-ldA" alt=""><figcaption></figcaption></figure>

* When an Inheritance will is activated, the listed beneficiaries will be added as new co-signers of the Safe wallet via the AddOwner transaction.&#x20;
* The minimum number of signatures required to execute a transaction in the Safe wallet is updated to the number set by the owner in the will via ChangeThreshold event.
* Will contract will emit AddOwner event and ChangeThreshold event. Subgraph will listen and update the list of owners of the Safe wallet, the updated threshold (minimum number of signatures required) and the will's status to activated.

#### Forwarding will

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcpAzjSBHv4gx5rhaInLqNm28RY2FIoVB52RzA05VoB_UTgIyPZ8CTYw4tfl4_i029h2ulqppfYO3EwNoav5OM7G3yaJpvKk3UiOHVJU5qr6IJpp2uvgwoVdwPUb08svhsh3S23?key=FU1CpoPnSDCdWQuIWr-ldA" alt=""><figcaption></figcaption></figure>

* When a Forwarding will is activated, the amount of ETH and ERC-20 tokens in the Safe wallet will be transferred to the beneficiaries' addresses based on pre-defined allocations.
* Will Contract will emit ForwardingActivated event. Subgraph will listen and update will's status to activated



