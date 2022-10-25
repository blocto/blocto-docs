---
description: Use Blocto wallet SDK to connect Ethereum
---

# Contract

{% hint style="warning" %}
Note that Blocto SDK for Ethereum-like chains is still in **Beta**.  
APIs are subject to breaking changes.
{% endhint %}

Install from npm/yarn

```bash
$ yarn add @blocto/sdk
```

### Use web3 api read from the contract

```javascript
import Web3 from "web3";
import BloctoSDK from "@blocto/sdk";

const bloctoSDK = new BloctoSDK({
  ethereum: {
    chainId: "0x4", // (required) chainId to be used
    rpc: `https://rinkeby.infura.io/v3/ef5a5728e2354955b562d2ffa4ae5305`, // (required for Ethereum) JSON RPC endpoint
  },
});

const web3 = new Web3(bloctoSDK.ethereum);

const handleGetContract = async () => {
  try {
    // required: contractAbi contractAddress
    const result = new web3.eth.Contract(contractAbi, contractAddress);
  } catch (error) {
    console.error("error", error);
  }
};
```

{% embed url="https://codesandbox.io/s/blocto-sdk-evm-contract-prg7rk?file=/src/App.js" %}
