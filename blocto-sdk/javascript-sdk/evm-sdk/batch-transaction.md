---
description: Use Blocto SDK to Combine multiple transactions and make them atomic
---

# Batch Transaction

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

### Introduction

With Blocto, you can combine multiple transactions into a single transaction for the following advantages:

1. Save gas fee
2. Make multiple transactions atomic, so they either all succeed or all fail

### Usage

There are two ways to combine transactions:

#### A. EIP-1193 (Recommended)

```javascript
import Web3 from 'web3';

// Use the Ethereum provider injected by Blocto app
const txHash = await bloctoSDK.ethereum.request({
  method: 'blocto_sendBatchTransaction',
  params: [
    web3.eth.sendTransaction.request(SOME_REQUEST),
    web3.eth.sendTransaction.request(SOME_OTHER_REQUEST)
  ]
})

console.log(txHash) // ex: 0x12a45b...
```

#### B. Web3 Batch Request

```javascript
import Web3 from 'web3';

// Use the Ethereum provider injected by Blocto app
const web3 = new Web3(bloctoSDK.ethereum);
const batch = new web3.BatchRequest();

batch.add(web3.eth.sendTransaction.request(SOME_REQUEST));
batch.add(web3.eth.sendTransaction.request(SOME_OTHER_REQUEST));

batch.execute();
```

### Example

#### Step 1 - Configure Web3 and @blocto/sdk

```javascript
import Web3 from "web3";
import BloctoSDK from "@blocto/sdk";

const bloctoSDK = new BloctoSDK({
  ethereum: {
    chainId: "0x5", // (required) chainId to be used
    rpc: `https://goerli.infura.io/v3/${YOUR_INFURA_ID}`, // (required for Ethereum) JSON RPC endpoint
  },
});

const web3 = new Web3(bloctoSDK.ethereum);

export { web3, bloctoSDK };
```

#### Step 2 - Send Batch Transaction

```javascript
const txHash = await bloctoSDK.ethereum.request({
  method: 'blocto_sendBatchTransaction',
  params: [
    web3.eth.sendTransaction.request(SOME_REQUEST),
    web3.eth.sendTransaction.request(SOME_OTHER_REQUEST)
  ]
})

console.log(txHash) // ex: 0x12a45b...
```

## Sample Code

{% embed url="https://codesandbox.io/p/sandbox/github/blocto/blocto-sdk-examples/tree/main/with-evm-blocto-batch-transaction?file=/src/App.js" %}

For more information about batch transactions, check out [web3.js documentation](https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html#batchrequest).
