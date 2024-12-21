# Wills created with EOAs

An **externally owned account (EOA)** is an account controlled by a cryptographic keypair and is most commonly referred to as a wallet. A key pair consists of a public key (a.k.a public address) and a private key. For **Forwarding** will, the user has an option to create a will using their own Ethereum address.

### Table of contents

[Create a will](./#create-a-will)

[Edit a will](./#edit-a-will)

[Delete a will](./#delete-a-will)

[Reset time to activation](./#reset-time-to-activation)

[Activate a will](./#activate-a-will)

### Create a will

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeETOc5ziQJ4Fzy6bA-oiCRYY1KympP8ScX07WfKYfloZDOn-vAlou3BchgaFiohEnbC3Hsfr1XymcX6lfBEqqbsiZ3LQIzc-tiS8BeZyCnGhZdQvwlWfnTdDvJnuJGSvQzQf8hiQ?key=odBWC6NT8SMxbDV561Yp6OAP" alt=""><figcaption></figcaption></figure>

* When the user create a new will, beneficiaries and allocations are required in the will contract. Once the will is created, the timestamp of the last outgoing transaction of the will owner's wallet is initialized
* The owner is required to approve the will contract the permission to transfer token from owner's wallet to beneficiaries when the will is activated.
* The owner is required to deposit ETH the native asset into the will contract, which will transfer ETH from the owner's wallet to the will contract. When the will is activated, the will contract will transfer ETH to beneficiaries.
* The Router Contract emits a ForwardingEOAWillCreated event and Subgraph listens and creates a WillDetail entity.

### Edit a will

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd2EVqoqAU1dAZqGzpd8xivOBBev8X4hpShpTYRWcsB3p8WPHNI2KrfHiOKUQm5lw8FNL46vLFy4XTV3grS2Jbfz4hQ5hARi8lfuUmBlIHDadkxQVTribdtS7TLn7jNBCLLE6pK?key=odBWC6NT8SMxbDV561Yp6OAP" alt=""><figcaption></figcaption></figure>

* When the owner edits a will, the new information is updated in the will contract, and the  timestamp of the last outgoing transaction in the will owner's is reset.
* When the owner edits the will to approve more tokens, the new token and/or new amount is updated in the will contract. This action alone will not reset the timestamp of the owner's wallet.
* When the owner deposits or withdraws ETH in or from the contract, the amount is updated, and the timestamp of the last outgoing transaction in the will owner's is reset.
* The Router Contract emits a ForwardingEOAWillUpdated event andSubgraph listens and updates the will contract with new information.

### Delete a will

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeCdvNTQfU2ukuB2bgHMHFz4L3hlMqWgaOPEoYrgIUHx4CJEn2KuSkPaPwgaZY0kssd3vTg9CUfYOozUrOkeNTYbmodTEpGE_ypumHVyu_nSJ9gc-DW9KpWWxJl52d-EUiYHUf6zw?key=odBWC6NT8SMxbDV561Yp6OAP" alt=""><figcaption></figcaption></figure>

* When the owner deletes an existing will, the Router Contract emits a ForwardingEOAWillDeleted event and Subgraph listens and updates the will's status to deleted.&#x20;
* The amount of previously deposited ETH in the contract will be transferred to the will owner.

### Reset time to activation

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXe2UwX4gwqN1YDfAD1iz7eHljWyzef2Zlv_X8zzPrGBTEfohmnPPhpkPNtc0LLlpsnvcVVG-cBI9o8XwCsHKMn2w24NmdR_Or8A7RsXHU4R4bp_M_VJ_8koPQQP33pJtfbuTs1plQ?key=odBWC6NT8SMxbDV561Yp6OAP" alt=""><figcaption></figcaption></figure>

* When the will's owner clicks the button "I'm still alive",  the system resets the timestamp of the last outgoing transaction.
* Router Contract emits a ForwardingEOAWillActiveAlive event and Subgraph listens and updates the last timestamp\


### Activate a will

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdmcx2YL5YevNcScZiDkaZD__kBTtHikw8tyfA01VWYawWVZ0HaWaFUYU88B4eoyc1PiY7M6meF8GN1Y5KMBNXZOOiOolzYTBHct4-zsauYlPk9L49hlINg9du6mARaXeQgFKe8eQ?key=odBWC6NT8SMxbDV561Yp6OAP" alt=""><figcaption></figcaption></figure>

* When a beneficiary checks a will's status, the Active Will function checks whether the will can be activated. If not enough time (as specified by the will) has passed since the last outgoing transaction, the will contract can’t be activated. Otherwise, the will contract can be activated by one of the beneficiary.
* Once the will is activated, the amount of ETH deposited in the will contract and the amount of tokens approved will be transferred to beneficiaries's wallets.
* Router Contract emits a ForwardingEOAWillActiveAlive event and Subgraph listens and updates the will's status to activated.
