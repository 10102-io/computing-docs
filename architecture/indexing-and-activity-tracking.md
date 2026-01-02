# Indexing & Activity Tracking

[The Graph](https://thegraph.com/) is a decentralized indexing protocol used to efficiently query and organize blockchain data that is otherwise difficult or expensive to access directly from smart contracts.

10102 uses The Graph to index and monitor on-chain data relevant to legacy contracts and timelocks including token balances and contract-related events.

### **Subgraphs**

We maintain multiple subgraphs with distinct responsibilities:

* **Subgraph 1**\
  Indexes data related to legacy contracts, timelocks, and reminder configurations.
* **Subgraph 2**\
  Indexes token balances (ERC-20, ERC-721, ERC-1155) associated with user wallets.
* **Subgraph 3**\
  Periodically aggregates the total ETH held under each legacy contract, as well as system-wide totals.

These subgraphs provide efficient read access for the application layer and user interface.

### **Wallet Activity & Activation Logic**

[Chainlink](https://chain.link/) is a decentralized oracle network that enables smart contracts to securely access off-chain data and external computation. 10102 uses Chainlink Functions to request, verify, and relay wallet activity data required to evaluate legacy contract activation conditions.

[Moralis](https://moralis.com/) is a blockchain data infrastructure platform that provides indexed access to on-chain transaction history across multiple networks. 10102 uses Moralis APIs to retrieve and analyze historical wallet transactions.

Due to current limitations in the EVM and Solidity, smart contracts cannot directly determine the timestamp of the most recent outgoing transaction from an arbitrary externally owned account (EOA), unless that wallet includes custom on-chain logic to track such activity.

To support activation based on wallet inactivity, 10102 uses a hybrid on-chain/off-chain approach:

* When a beneficiary attempts to activate a legacy contract, the system invokes a **Chainlink Function**.
* The Chainlink Function queries **Moralis APIs** to retrieve the transaction history of the contract owner’s wallet.
* The system identifies the timestamp of the most recent outgoing transaction.
* This timestamp is compared against the inactivity period defined in the legacy contract.
* If the required inactivity threshold has been met, the activation proceeds and the contract executes the transfer of funds to the beneficiary’s wallet.

Even though the activity check happens via Chainlink, the final authority remains the smart contract, which is subjected to predefined rules and cannot be arbitrarily bypassed.

This design ensures activation conditions are evaluated reliably while respecting the technical constraints of on-chain execution.

