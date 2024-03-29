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

#### Goerli RPC demo

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

### Step 2 - Connect / Disconnect

```javascript
const loginHandler = async () => {
  const accounts = await bloctoSDK.ethereum.request({
    method: "eth_requestAccounts"
  });
  setAddress(accounts[0]);
};

const logoutHandler = async () => {
  try {
    await bloctoSDK.ethereum.request({ method: "wallet_disconnect" });
    localStorage.removeItem("sdk.session");
    setAddress(null);
  } catch (error) {
    console.log(error);
  }
};
```

#### Connect with prefilled email

```javascript
const loginWithPrefilledEmailHandler = async () => {
  const accounts = await bloctoSDK?.ethereum?.request({
    method: "eth_requestAccounts",
    params: ["client@email.com"] // When the email is provided, the login modal will prefill the email field.
  });
  setAddress(accounts[0]);
};
```

## Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk-examples/tree/main/src/evm/with-blocto-connect?file=/src/App.js" %}
