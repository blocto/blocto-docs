---
description: Use Blocto wallet SDK with your existing web3.js integration
---

# Web3.js Integration

You can use Blocto wallet service with your current [web3.js](https://web3js.readthedocs.io/en/v1.3.4/) integration with only a few simple lines of code. 

{% hint style="warning" %}
Note that Blocto SDK for Ethereum-like chains is still in **Beta**.  
APIs are subject to breaking changes.
{% endhint %}

```bash
$ yarn add web3
$ yarn add @blocto/sdk
```

```javascript
import Web3 from 'web3'
import BloctoProvider from '@blocto/sdk'

// Initiate the Blocto provider for web3.js
const bloctoProvider = new BloctoProvider({
    // (required) chainId to be used
    chainId: '0x1', 
    
    // (required for Ethereum) JSON RPC endpoint
    rpc: 'https://mainnet.infura.io/v3/YOUR_INFURA_ID',
    
    // (optional) Blocto app ID
    appId: 'YOUR_BLOCTO_APP_ID',
});

// use as web3 provider
const web3 = new Web3(bloctoProvider);
```

You can then use the `web3` object for on-chain interactions.  For more documentation for what you can do with web3.js, check out 

* [web3.js 1.2.x docs](https://web3js.readthedocs.io/en/v1.2.11/index.html)
* [web3.js 0.2x.x docs](https://github.com/ethereum/web3.js/blob/0.20.7/DOCUMENTATION.md)

