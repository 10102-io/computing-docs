# Architecture

## Using a Safe wallet:

A Safe Wallet is a multi-signature smart account management tool on Ethereum that allows users to manage their crypto assets. A predefined number of private keys is required to sign off on any transaction, so no single person can move the funds without the consent of others.&#x20;

| ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd023Y5T85v3t-wdW8_Ibhcs3hr-jnc9si2jJKDEcC1GwVpzEvm3fcyEjq3gb6jhkVMPhPv-VJlnUmIlVCiQ1F26PiWNro4tWk5x-q0PjU8nUaVbdryAuXe8Zv-PVqGZeuAoYEIQQ?key=FU1CpoPnSDCdWQuIWr-ldA) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

| ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeP_Mtt8Mt7hrEHu3mB7ODtwwC5A6ywCCKJNDlMHdFwAW6xehP5cAklLsFFjol9BcyB_jo0dOXx5L6_3CjyaznZ7KvrMfqGptvKfSWqr6Jqlmr1KaAn_QNoWSrw2nSstDMxADsh?key=FU1CpoPnSDCdWQuIWr-ldA) |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdhYB4xXaD6WdypcO0h-RSou1Em0-QsnHqSW9Y7ODdoyv4EDG_8OIu78Cmluc3_jUdXzKeOVBn6T_iCFG7XqE7gKKQWwKVZK9z5uS4MyBPNxQmjAQio-g2fE9IEq878LKVMTqsa?key=FU1CpoPnSDCdWQuIWr-ldA) |

### Create a new Safe Wallet:

* Step 1: Open:[ https://app.safe.global/new-safe/create](https://app.safe.global/new-safe/create)
* Step 2: Connect wallet and fill the name of your Safe wallet and the network that you want to use.
* Step 3: Set the list of signers  and set the minimum signature number of the safe wallet (threshold)
* Step 4: Sign and execute to create a new Safe account

### Execute a transaction using Safe Wallet:

In order to execute a transaction using a Safe wallet, the co-signers will have to sign the transaction and meet the  minimum number of signatures set by the Safe wallet's owner.

* Step 1: Open you safe wallet account on site: [https://app.safe.global](https://app.safe.global) -> new transaction -> Transaction Builder
* Step 2: Fill in the address and ABI of the smart contract that you want to execute.
* Step 3: Select method and fill the parameters of method
* Step 4: Sign and execute transaction

### Set guard and module in Safe wallet:

* Step 1: Open you safe wallet account on site: [https://app.safe.global](https://app.safe.global) -> New transaction -> Transaction Builder
* Step 2: Fill in the your Safe wallet address -> Use Implementation ABI
* Step 3: Select method: setGuard and fill in the guard address into parameter -> add new transaction
* Step 4: Select method: enableModule and fill the will address into parameter -> add new transaction
* Step 5: Sign and execute transaction

### Create will

* Step 1:  Create will => Safe guard and Safe module(will contract) is created&#x20;
* Step 2: the Safe guard and the Safe module is attached to your Safe wallet => last timestamp of the will owner is initialized into Safe guard

### Edit Will

* Edit will => the will information is updated into Safe module(will contract) => last timestamp of will owner is updated into Safe guard

### Delete Will

* Delete will => Safe guard and safe module (will contract) is removed from your Safe wallet&#x20;

### Active Will

* Active Will checks whether the will can be activated. If not enough time (as specified by the will) has passed since the last outgoing transaction, the will contract can’t be activated. Otherwise, the will contract can be activated by one of the beneficiary.
* When an Inheritance will is activated, the listed beneficiaries  will be added as new co-signers of the Safe wallet. The same number of minimum signatures to execute a transaction is required as set previously by the Safe wallet.
* When a Forwarding will is activated, the amount of ETH and tokens in the safe wallet will be transferred to beneficiaries based on pre-defined allocations.

Using an Externally Owned Account (EOA):\



| ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXctjcYFMA8_AppgMKotPjN4zdZRtQrVsUDxwy5rRh1Yhq11Hb9K403BzNqKOYmR7AZxxBqfOElEJO5rRnNd3p-faR93mg9T16RLCCRdtA4q65DvkcxFbZXDF0RacmMmuV_Ao_vrMg?key=odBWC6NT8SMxbDV561Yp6OAP) |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Create will

* Step 1:  When the user create a new will, beneficiaries and allocations are required in the will contract. Once the will is created, the timestamp of the last outgoing transaction of the will owner's wallet is initialized
* Step 2:  Approve token - the user is required to approve the will contract the permission to transfer token from owner's wallet to beneficiaries when the will is activated.
* Step 3: Deposit ETH - the user is required to deposit ETH the native asset into the will contract, which will transfer ETH from the owner's wallet to the will contract. When the will is activated, the will contract will transfer ETH to beneficiaries.

### Edit Will

* Step 1: Edit will => the will information is updated => the  timestamp of the last outgoing transaction in the will owner's is reset.
* Step 2: Edit approve token => the amount token is updated. This action will not reset the timestamp of the owner's wallet.
* Step 3: Deposit or withdraw ETH => the  timestamp of the last outgoing transaction in the will owner's is reset.

### Delete Will

* Step 1: Delete will => the will is deleted
* Step 2:  The total amount of ETH in the contract will be transferred to the will owner => last timestamp of will owner is updated&#x20;

### Active Alive

* Step 1: If the will's owner clicks the button "I'm still alive",  the system that will reset the timestamp of will owner's last outgoing transaction.

### Active Will

* Active Will checks whether the will can be activated. If not enough time (as specified by the will) has passed since the last outgoing transaction, the will contract can’t be activated. Otherwise, the will contract can be activated by one of the beneficiary.
* Once the will is activated, the amount of ETH deposited in the will contract and the amount of tokens approved will be transferred to beneficiaries's wallets.
