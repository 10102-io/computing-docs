# Legacy Contracts Created with EOAs

An **externally owned account (EOA)** is an account controlled by a cryptographic keypair and is most commonly referred to as a wallet. A key pair consists of a public key (a.k.a public address) and a private key. For **Transfer Legacy** contracts, the user has an option to create a will using their own Ethereum address.

### Table of contents

[Create a Legacy Contract](legacy-contracts-created-with-eoas.md#create-a-legacy-contract)

[Edit a Legacy Contract](legacy-contracts-created-with-eoas.md#edit-a-legacy-contract)

[Delete a Legacy Contract](legacy-contracts-created-with-eoas.md#delete-a-legacy-contract)

[Reset Time to Activation](legacy-contracts-created-with-eoas.md#reset-time-to-activation)

[Activate a Legacy Contract](legacy-contracts-created-with-eoas.md#activate-a-legacy-contract)

### Create a Legacy Contract <a href="#create-a-legacy-contract" id="create-a-legacy-contract"></a>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeETOc5ziQJ4Fzy6bA-oiCRYY1KympP8ScX07WfKYfloZDOn-vAlou3BchgaFiohEnbC3Hsfr1XymcX6lfBEqqbsiZ3LQIzc-tiS8BeZyCnGhZdQvwlWfnTdDvJnuJGSvQzQf8hiQ?key=odBWC6NT8SMxbDV561Yp6OAP" alt=""><figcaption></figcaption></figure>

* When the user create a new legacy contract, beneficiaries and allocations are required. Once created, the contract will initialize the timestamp of the last outgoing transaction of the owner's wallet.
* The owner is required to approve the legacy contract the permission to transfer token from the owner's wallet to beneficiaries when the contract is activated.
* The owner is required to deposit ETH the native asset into the legacy contract, which will transfer ETH from the owner's wallet to the legacy contract. When the legacy contract is activated, the contract will transfer the ETH to beneficiaries.
* The Router Contract emits a event and Subgraph listens and creates a legacy contract entity.

### Edit a Legacy Contract <a href="#edit-a-legacy-contract" id="edit-a-legacy-contract"></a>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd2EVqoqAU1dAZqGzpd8xivOBBev8X4hpShpTYRWcsB3p8WPHNI2KrfHiOKUQm5lw8FNL46vLFy4XTV3grS2Jbfz4hQ5hARi8lfuUmBlIHDadkxQVTribdtS7TLn7jNBCLLE6pK?key=odBWC6NT8SMxbDV561Yp6OAP" alt=""><figcaption></figcaption></figure>

* When the owner edits a legacy contract, the new information is updated in the legacy contract, and the timestamp of the last outgoing transaction in the contract's owner's wallet is reset.
* When the owner edits the legacy contract to approve more tokens, the new tokens and/or new amount is updated in the legacy contract. This action alone will not reset the timestamp of the owner's wallet.
* When the owner deposits or withdraws ETH in or from the contract, the amount is updated, and the timestamp of the last outgoing transaction in the legacy contract owner's wallet is reset.
* The Router Contract emits a updated event and Subgraph listens and updates the legacy contract with new information.

### Delete a Legacy Contract <a href="#delete-a-legacy-contract" id="delete-a-legacy-contract"></a>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeCdvNTQfU2ukuB2bgHMHFz4L3hlMqWgaOPEoYrgIUHx4CJEn2KuSkPaPwgaZY0kssd3vTg9CUfYOozUrOkeNTYbmodTEpGE_ypumHVyu_nSJ9gc-DW9KpWWxJl52d-EUiYHUf6zw?key=odBWC6NT8SMxbDV561Yp6OAP" alt=""><figcaption></figcaption></figure>

* When the owner deletes an existing legacy contract, the Router Contract emits a new event and Subgraph listens and updates the legacy contract' status to deleted.&#x20;
* The amount of previously deposited ETH in the contract will be transferred back to the owner's wallet.

### Reset Time to Activation

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXe2UwX4gwqN1YDfAD1iz7eHljWyzef2Zlv_X8zzPrGBTEfohmnPPhpkPNtc0LLlpsnvcVVG-cBI9o8XwCsHKMn2w24NmdR_Or8A7RsXHU4R4bp_M_VJ_8koPQQP33pJtfbuTs1plQ?key=odBWC6NT8SMxbDV561Yp6OAP" alt=""><figcaption></figcaption></figure>

* When the legacy contract owner clicks the button "I'm still alive",  the system resets the timestamp of the last outgoing transaction.
* Router Contract emits a new event and Subgraph listens and updates the last timestamp of the contract owner's wallet.<br>

### Activate a Legacy Contract <a href="#activate-a-legacy-contract" id="activate-a-legacy-contract"></a>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdmcx2YL5YevNcScZiDkaZD__kBTtHikw8tyfA01VWYawWVZ0HaWaFUYU88B4eoyc1PiY7M6meF8GN1Y5KMBNXZOOiOolzYTBHct4-zsauYlPk9L49hlINg9du6mARaXeQgFKe8eQ?key=odBWC6NT8SMxbDV561Yp6OAP" alt=""><figcaption></figcaption></figure>

* When a beneficiary checks a legacy contract's status, a function checks whether the legacy contract can be activated. If not enough time (as specified by the legacy contract) has passed since the last outgoing transaction, the legacy contract can’t be activated. When the time requirement is met, the legacy contract can be activated by one of the beneficiaries.
* Once the legacy contract is activated, the amount of ETH deposited in the legacy contract and the amount of tokens approved will be transferred to the designated beneficiaries's wallets.
* Router Contract emits a new event and Subgraph listens and updates the legacy contract's status to activated.
