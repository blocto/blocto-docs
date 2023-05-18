---
description: Use Blocto SDK to Combine multiple transactions and make them atomic
---

# Batch Transaction

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

{% hint style="info" %}
Visit [batch-transaction.md](../../../blocto-app/web3-provider/batch-transaction.md "mention") for more usage and example.
{% endhint %}

### Introduction

1. Save gas fee
2. Make multiple transactions atomic, so they either all succeed or all fail

### Usage

#### Step 1 - Configure Web3 and @blocto/sdk

```javascript
import Web3 from "web3";
import BloctoSDK from "@blocto/sdk";

const bloctoSDK = new BloctoSDK({
  ethereum: {
    chainId: "0x5", // (required) chainId to be used
    rpc: `https://goerli.infura.io/v3/ef5a5728e2354955b562d2ffa4ae5305`, // (required for Ethereum) JSON RPC endpoint
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
    ...web3.eth.sendTransaction.request(SOME_REQUEST).params,
    ...web3.eth.sendTransaction.request(SOME_OTHER_REQUEST).params
  ]
})

console.log(txHash) // ex: 0x12a45b...
```

### Sample

{% embed url="https://codesandbox.io/s/evm-batch-transaction-45g0c3?file=/package.json" %}

For more information about batch transactions, check out [web3.js documentation](https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html#batchrequest).
