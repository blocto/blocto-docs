---
description: Use Blocto wallet SDK to connect Ethereum
---

# Login / Register

{% hint style="warning" %}
Note that Blocto SDK for Ethereum-like chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

Install from npm/yarn

```bash
$ yarn add @blocto/sdk
```

### Step 1 - Configure Web3 and @blocto/sdk

#### Rinkeby RPC demo

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

export { web3, bloctoSDK };
```

### Step 2 - Authenticate

#### Authentication demo

```javascript
const loginHandler = async () => {
  const accounts = await bloctoSDK?.ethereum?.enable();
  setAddress(accounts[0]);
};

const logoutHandler = async () => {
  try {
    await bloctoSDK?.ethereum?.request({ method: "wallet_disconnect" });
    localStorage.removeItem("sdk.session");
    setAddress(null);
  } catch (error) {
    console.log(error);
  }
};
```

{% embed url="https://codesandbox.io/s/blocto-sdk-oytrrg?file=/src/App.js" %}
