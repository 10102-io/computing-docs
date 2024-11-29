# Architecture

Safe wallet:\
\



| ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd023Y5T85v3t-wdW8_Ibhcs3hr-jnc9si2jJKDEcC1GwVpzEvm3fcyEjq3gb6jhkVMPhPv-VJlnUmIlVCiQ1F26PiWNro4tWk5x-q0PjU8nUaVbdryAuXe8Zv-PVqGZeuAoYEIQQ?key=FU1CpoPnSDCdWQuIWr-ldA) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\


| ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeP_Mtt8Mt7hrEHu3mB7ODtwwC5A6ywCCKJNDlMHdFwAW6xehP5cAklLsFFjol9BcyB_jo0dOXx5L6_3CjyaznZ7KvrMfqGptvKfSWqr6Jqlmr1KaAn_QNoWSrw2nSstDMxADsh?key=FU1CpoPnSDCdWQuIWr-ldA) |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdhYB4xXaD6WdypcO0h-RSou1Em0-QsnHqSW9Y7ODdoyv4EDG_8OIu78Cmluc3_jUdXzKeOVBn6T_iCFG7XqE7gKKQWwKVZK9z5uS4MyBPNxQmjAQio-g2fE9IEq878LKVMTqsa?key=FU1CpoPnSDCdWQuIWr-ldA) |

\


### Create Safe Wallet:

The safe wallet is a smart contract wallet. The owner of the safe wallet includes a list of signers. If you want to execute a transaction by a safe wallet, the signers will have to sign this transaction until enough the minimum number of signatures of the safe wallet.

* Step1: Open on site:[ https://app.safe.global/new-safe/create](https://app.safe.global/new-safe/create)
* Step2: Connect wallet and fill the name of your safe wallet and the network that you want to use.
* Step 3: Set the list of signers  and set the minimum signature number of the safe wallet (threshold)
* Step 4: Sign and execute create account transaction

### Execute transaction safe wallet:

* Step 1: Open you safe wallet account on site: [https://app.safe.global](https://app.safe.global) -> new transaction -> Transaction Builder
* Step 2: Fill in the address and abi of the smart contract that you want to execute.
* Step 3: Select method and fill the parameters of method
* Step 4: Sign and execute transaction

### Set guard and module safe wallet:

* Step 1: Open you safe wallet account on site: [https://app.safe.global](https://app.safe.global) -> new transaction -> Transaction Builder
* Step 2: Fill in the your safe wallet address -> Use Implementation ABI
* Step 3: Select method: setGuard and fill in the guard address into parameter -> add new transaction
* Step 4: Select method: enableModule and fill the will address into parameter -> add new transaction
* Step 5: Sign and execute transaction

### Create will

* Step 1:  Create will => safe guard and safe module(will contract) is created&#x20;
* Step 2: the safe guard and the safe module is attached to your safe wallet => last timestamp of the will owner is initialized into safe guard

### Edit Will

* Step 1: Edit will => the will information is updated into safe module(will contract) => last timestamp of will owner is updated into safe guard

### Delete Will

* Step 1: Delete will => safe guard and safe module (will contract) is removed from your safe wallet&#x20;

### Active Will

* Step 1: Active Will => check whether the will owner is alive. If alive, the will contract can’t be activated. Otherwise, the will contract activated&#x20;
* Step 2: In inheritance case: list of beneficiaries are gonna be new signers of the safe wallet. In forwarding case: the amount of ETH and token in the safe wallet will be transferred to beneficiaries

## Externally Owned Account (EOA):



\


| ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXctjcYFMA8_AppgMKotPjN4zdZRtQrVsUDxwy5rRh1Yhq11Hb9K403BzNqKOYmR7AZxxBqfOElEJO5rRnNd3p-faR93mg9T16RLCCRdtA4q65DvkcxFbZXDF0RacmMmuV_Ao_vrMg?key=odBWC6NT8SMxbDV561Yp6OAP) |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\
\


### Create will

* Step 1:  Create will => the newly created will with will information => last timestamp of the will owner is initialized
* Step 2:  Approve token => approve the will contract the permission to transfer token from owner to beneficiaries when activated will.
* Step 3: Deposit ETH => transfer ETH from owner wallet to the will contract that will contract can transfer ETH to beneficiaries when activated will

### Edit Will

* Step 1: Edit will => the will information is updated => last timestamp of will owner is updated
* Step 2: Edit approve token => the allowance token is updated
* Step 3: Deposit or withdraw ETH => last timestamp of will owner is updated

### Delete Will

* Step 1: Delete will => the will is delete&#x20;
* Step 2:  The total amount of ETH in the contract will be transferred to the will owner => last timestamp of will owner is updated&#x20;

### Active Alive

* Step1: Active alive => Inform the system that will owner will be alive => last timestamp of will owner is updated

### Active Will

* Step 1: Active Will => check whether the will owner is alive. If alive, the will contract can’t be activated. Otherwise, the will contract activated&#x20;
* Step 2: The amount of ETH in the will contract and the amount of token is approved will be transferred to beneficiaries
