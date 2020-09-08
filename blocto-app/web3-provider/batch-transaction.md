---
description: Combine multiple transactions and make them atomic
---

# Batch Transaction

### Introduction

With Blocto, you can combine multiple transactions into a single transaction for the following advantages:

1. Save gas fee
2. Make multiple transactions atomic, so they either all succeed or all fail

### Usage

```javascript
import Web3 from 'web3';

// Use the Ethereum provider injected by Blocto app
const web3 = new Web3(window.ethereum);
const batch = new web3.BatchRequest();

batch.add(web3.eth.sendTransaction.request(SOME_REQUEST));
batch.add(web3.eth.sendTransaction.request(SOME_OTHER_REQUEST));

batch.execute();
```

For example, if you are building a campaign for PoolTogether. You want to let user claim a DAI token from a smart contract, approve PoolTogether from spending user's DAI and deposit the DAI into PoolTogether, you can do something like:

```javascript
import Web3 from 'web3';

// Use the Ethereum provider injected by Blocto app
const web3 = new Web3(window.ethereum);
const batch = new web3.BatchRequest();

// claim DAI from some promotion smart contract
batch.add(web3.eth.sendTransaction.request({
  from: address,
  to: 'SOME_PROMOTION_CONTRACT',
  data: 'SOME_METHOD_HASH',
}, 'latest'));

// approve DAI
batch.add(web3.eth.sendTransaction.request({
  from: address,
  to: '0x6b175474e89094c44da98b954eedeac495271d0f',
  data: '0x095ea7b300000000000000000000000029fe7d60ddf151e5b52e5fab4f1325da6b2bd9580000000000000000000000000000000000000000000845951614014849ffffff',
}, 'latest'));

// put in PoolTogether
batch.add(web3.eth.sendTransaction.request({
  from: address,
  to: '0x29fe7D60DdF151E5b52e5FAB4f1325da6b2bD958',
  data: '0x234409440000000000000000000000000000000000000000000000000de0b6b3a7640000',
}, 'latest'));

batch.execute();
```

For more information about batch transactions, check out [web3.js documentation](https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html#batchrequest).

