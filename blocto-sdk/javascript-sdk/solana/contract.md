---
description: Use Blocto wallet SDK to connect Solana
---

# Contract

{% hint style="warning" %}
Note that Blocto SDK for Ethereum-like chains is still in **Beta**.  
APIs are subject to breaking changes.
{% endhint %}

Install from npm/yarn

```bash
$ yarn add @blocto/sdk @solana/web3.js @solana/buffer-layout
```

### Use solana/web3.js api read from the contract

```javascript
import BloctoSDK from "@blocto/sdk";
import { PublicKey } from "@solana/web3.js";
import * as BufferLayout from "@solana/buffer-layout";

const bloctoSDK = new BloctoSDK({
  solana: {
    net: "devnet",
  },
});

const handleGetContract = async () => {
  const formatProgramStruct = (data) => {
    const withBufferLayout = data.map((attribute) =>
      BufferLayout?.[attribute.type]?.(attribute.name)
    );
    return BufferLayout.struct(withBufferLayout);
  };

  const getContract = async () => {
    try {
      const key = new PublicKey(accountPubKey);
      // Different response type than the type of the response from getAccountInfo in @solana/web3.js
      const accountInfo = await bloctoSDK?.solana?.request({
        method: "getAccountInfo",
        params: [
          key,
          {
            encoding: "base64"
          }
        ]
      });

      if (!accountInfo) {
        setResponse("Error: Program not found.");
      }
      const layout = formatProgramStruct(JSON.parse(struct));
      const info = layout.decode(accountInfo?.data);
      setResponse(info);
    } catch (error) {
      console.error("error", error);
      setResponse(error);
    }
  };
```

{% embed url="https://codesandbox.io/s/blocto-sdk-evm-contract-prg7rk?file=/src/App.js" %}
