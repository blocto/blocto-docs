---
description: Use Blocto wallet SDK to connect Ethereum
---

# Connect / Disconnect

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
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

### Step 2 - Connect / Disconnect

```javascript
const loginHandler = async () => {
  // (optional) when the email is provided, the login modal will prefill the email field.
  const accounts = await bloctoSDK?.ethereum?.enable("client@email.com");
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

{% embed url="https://codesandbox.io/s/blocto-sdk-y1l23o?file=/src/App.js" %}
