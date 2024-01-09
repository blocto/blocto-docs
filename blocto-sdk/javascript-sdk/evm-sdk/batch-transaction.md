---
description: Use Blocto SDK to Combine multiple transactions and make them atomic
---

# Batch Transaction

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

{% hint style="info" %}
Blocto SDK now supports [Web3.js v4 batch transactions](https://docs.web3js.org/guides/web3\_upgrade\_guide/x/#web3-batchrequest) with **version >= v.0.9.0**. \
For version < v0.9.0, please use it along with Web3.js v1.
{% endhint %}

### Introduction

With Blocto, you can combine multiple transactions into a single transaction for the following advantages:

1. Save gas fee
2. Make multiple transactions atomic, so they either all succeed or all fail

### Usage

There are two ways to combine transactions:

#### A. EIP-1193 (Recommended)

```javascript
import Web3 from "web3";

// Use the Ethereum provider injected by Blocto app
const txHash = await bloctoSDK.ethereum.request({
  method: "wallet_sendMultiCallTransaction",
  params: [
    [
      ...web3.eth.sendTransaction.request(SOME_REQUEST).params,
      ...web3.eth.sendTransaction.request(SOME_OTHER_REQUEST).params,
    ],
    false, //  (optional) revert flag default is false
  ],
});

console.log(txHash); // ex: 0x12a45b...
```

#### B. Web3.js Batch Request

#### web3.js 4.x.x version:

```javascript
import Web3 from "web3";

// Use the Ethereum provider injected by Blocto app
const web3 = new Web3(bloctoSDK.ethereum);
const batch = new web3.BatchRequest();
const request1 = {
  jsonrpc: "2.0",
  id: 10,
  method: "eth_getBalance",
  params: ["0xf4ffff492596ac13fee6126846350433bf9a5021", "latest"],
};
const request2 = {
  jsonrpc: "2.0",
  id: 12,
  method: "eth_getBalance",
  params: ["0xdc6bad79dab7ea733098f66f6c6f9dd008da3258", "latest"],
};
batch.add(request1);
batch.add(request2);

const txHash = await batch.execute();

console.log(txHash); // ex:  [{ "id":"10", "jsonrpc":"2.0", "method":"eth_getBalance", "result":"0x0" }, ...]
```

## Sample Code

{% embed url="https://codesandbox.io/p/sandbox/github/blocto/blocto-sdk-examples/tree/main/with-evm-blocto-batch-transaction-v4?file=/src/App.js" %}

For more information about batch transactions [web3.js 4.x.x documentation](https://docs.web3js.org/guides/web3\_upgrade\_guide/x/#web3-batchrequest).

#### web3.js 1.x.x version:

```javascript
import Web3 from "web3";

// Use the Ethereum provider injected by Blocto app
const web3 = new Web3(bloctoSDK.ethereum);
const batch = new web3.BatchRequest();

batch.add(web3.eth.sendTransaction.request(SOME_REQUEST));
batch.add(web3.eth.sendTransaction.request(SOME_OTHER_REQUEST));

batch.execute();
```

## Sample Code

{% embed url="https://codesandbox.io/p/sandbox/github/blocto/blocto-sdk-examples/tree/main/with-evm-blocto-batch-transaction?file=/src/App.js" %}

For more information about batch transactions, check out [web3.js documentation](https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html#batchrequest).
