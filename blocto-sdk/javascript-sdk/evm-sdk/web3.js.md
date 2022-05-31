---
description: Use Blocto wallet SDK with your existing web3.js integration
---

# Web3.js Integration

You can use Blocto wallet service with your current [web3.js](https://web3js.readthedocs.io/en/v1.3.4/) integration with only a few simple lines of code.&#x20;

{% hint style="warning" %}
Note that Blocto SDK for Ethereum-like chains is still in **Beta**.\
APIs are subject to breaking changes.
{% endhint %}

```bash
$ yarn add web3
$ yarn add @blocto/sdk^@0.2.1
```

```javascript
import Web3 from 'web3'
import BloctoSDK from '@blocto/sdk'

const bloctoSDK = new BloctoSDK({
    ethereum: {
        // (required) chainId to be used
        chainId: '0x1', 
        // (required for Ethereum) JSON RPC endpoint
        rpc: 'https://mainnet.infura.io/v3/YOUR_INFURA_ID',
    },
    
    // (optional) Blocto app ID
    appId: 'YOUR_BLOCTO_APP_ID',
});

const web3 = new Web3(bloctoSDK.ethereum);
```

| Parameter          | Type         | Description                                                                                            | Required                    |
| ------------------ | ------------ | ------------------------------------------------------------------------------------------------------ | --------------------------- |
| `ethereum.chainId` | String (hex) | <p>EVM chain ID to connect to</p><p>Reference: <a href="https://chainid.network/">EVM Networks</a></p> | **Yes**                     |
| `ethereum.rpc`     | String       | JSON RPC endpoint                                                                                      | **Yes** (only for Ethereum) |
| `appId`            | String       | Blocto dApp ID                                                                                         | **No**                      |

{% tabs %}
{% tab title="Ethereum Mainnet" %}
```javascript
const bloctoSDK = new BloctoSDK({
    ethereum: {
        chainId: '0x1', // 1
        rpc: 'https://mainnet.infura.io/v3/YOUR_INFURA_ID',
    },
    appId: 'YOUR_BLOCTO_APP_ID',
});
```
{% endtab %}

{% tab title="Ethereum Testnet (Rinkeby)" %}
```javascript
const bloctoSDK = new BloctoSDK({
    ethereum: {
        chainId: '0x4', // 4
        rpc: 'https://rinkeby.infura.io/v3/YOUR_INFURA_ID',
    },
    appId: 'YOUR_BLOCTO_APP_ID',
});
```
{% endtab %}

{% tab title="BSC Mainnet" %}
```javascript
const bloctoSDK = new BloctoSDK({
    ethereum: {
        chainId: '0x38', // 56
    },
    appId: 'YOUR_BLOCTO_APP_ID',
});
```
{% endtab %}

{% tab title="BSC Testnet (Chapel)" %}
```javascript
const bloctoSDK = new BloctoSDK({
    ethereum: {
        chainId: '0x61', // 97
    },
    appId: 'YOUR_BLOCTO_APP_ID',
});
```
{% endtab %}
{% endtabs %}

You can then use the `web3` object for on-chain interactions.  For more documentation for what you can do with web3.js, check out&#x20;

* [web3.js 1.2.x docs](https://web3js.readthedocs.io/en/v1.2.11/index.html)
* [web3.js 0.2x.x docs](https://github.com/ethereum/web3.js/blob/0.20.7/DOCUMENTATION.md)

### **Migrate from 0.1.x**

Start from 0.2.0 we make the ethereum provider and its corresponding config standalone, so that we can integrate with more other chains.

```javascript
// current version
const ethereumConfig = { chainId, rpc };
const sdk = new BloctoSDK({ ethereum: ethreumConfig, appId, /* other configs */ });
const web3 = new Web3(sdk.ethereum);

// 0.1.x
const config = { chainId, rpc, appId };
const provider = new BloctoProvider(config);
const web3 = new Web3(provider);
```

****
