---
description: Use Blocto wallet SDK with your existing web3.js integration
---

# Integrate with Web3.js

You can use Blocto wallet service with your current [web3.js](https://web3js.readthedocs.io/en/v1.3.4/) integration with only a few simple lines of code.

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i web3 @blocto/sdk
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add web3 @blocto/sdk
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add web3 @blocto/sdk
```
{% endtab %}
{% endtabs %}

### Usage

#### Initiate Blocto SDK and Web3

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

#### Blocto SDK Parameters

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

You can then use the `web3` object for on-chain interactions. For more documentation for what you can do with web3.js, check out

* [web3.js 1.2.x docs](https://web3js.readthedocs.io/en/v1.2.11/index.html)
* [web3.js 0.2x.x docs](https://github.com/ethereum/web3.js/blob/0.20.7/DOCUMENTATION.md)

#### Supported Chain ID

| Network                  | Chain ID |
| ------------------------ | -------- |
| Ethereum Mainnet         | 1        |
| Ethereum Rinkeby Testnet | 4        |
| Arbitrum Mainnet         | 42161    |
| Arbitrum Goerli Testnet  | 421613   |
| Optimism Mainnet         | 10       |
| Optimism Goerli Testnet  | 420      |
| Polygon Mainnet          | 137      |
| Polygon Mumbai Testnet   | 80001    |
| BSC Mainnet              | 56       |
| BSC Chapel Testnet       | 97       |
| Avalanche Mainnet        | 43114    |
| Avalanche Fuji Testnet   | 43113    |

***
