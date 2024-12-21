# Indexing via The Graph

[The Graph](https://thegraph.com/) is a decentralized infrastructure for indexing off-chain data.&#x20;

We use The Graph track which tokens are available in the owner's wallet and the timestamp of the last outgoing transaction. Due to EVM and Solidity’s current design, it’s impossible to retrieve the information about the last outgoing activity of an arbitrary wallet from a smart contract unless there's a special implementation in the wallet.&#x20;

You can take a look at our subgraph [here](https://thegraph.com/explorer/subgraphs/AhZZ8eBUPi7cYqPJPQ3Rq1TWJVAYxuJt2zcjzcYuC92j?view=Query\&chain=arbitrum-one):

