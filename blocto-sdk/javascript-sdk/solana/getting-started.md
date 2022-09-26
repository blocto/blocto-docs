---
description: A thin JSON-RPC wrapper for interacting with chains and Blocto wallet.
---

# Getting started

Blocto SDK comes with an [EIP-1193](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1193.md) compatible provider, you can use it to interact with Solana.

{% hint style="warning" %}
Note that Blocto SDK for Solana is still in **Beta**.\
APIs are subject to breaking changes.
{% endhint %}

### Installation

Install from npm/yarn

```bash
$ yarn add @solana/web3.js
$ yarn add @blocto/sdk
```

... or via CDN

```javascript
<script src="https://unpkg.com/@solana/web3.js@latest/lib/index.iife.min.js"></script>
<script src="https://unpkg.com/@blocto/sdk" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
```

### **Usage**

Initiate the Blocto provider&#x20;

```javascript
import solanaWeb3 from '@solana/web3'
import BloctoSDK from '@blocto/sdk'

const bloctoSDK = new BloctoSDK({
    solana: {
        // (required) devnet to be used
        net: 'testnet',
        // (optional) rpc endpoint
        rpc: 'https://api.${net}.solana.com',
    },
    
    // (optional) Blocto app ID
    appId: 'YOUR_BLOCTO_APP_ID',
});
```

#### Blocto Provider parameters

| solana.net | String | one of `testnet` or `mainnet-beta` | **Yes** |
| ---------- | ------ | ---------------------------------- | ------- |
| solana.rpc | String | custom rpc endpoint                | **No**  |
| `appId`    | String | Blocto dApp ID                     | **No**  |

