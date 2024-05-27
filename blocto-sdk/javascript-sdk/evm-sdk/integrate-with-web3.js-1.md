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

<table><thead><tr><th width="216">Parameter</th><th width="130">Type</th><th width="250">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>ethereum.chainId</code></td><td>String (hex)</td><td><p>EVM chain ID to connect to</p><p>Reference: <a href="https://chainid.network/">EVM Networks</a></p></td><td><strong>Yes</strong></td></tr><tr><td><code>ethereum.rpc</code></td><td>String</td><td>JSON RPC endpoint</td><td><strong>Yes</strong> (only for Ethereum)</td></tr><tr><td><code>appId</code></td><td>String</td><td>Blocto dApp ID</td><td><strong>No</strong></td></tr></tbody></table>

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

{% tab title="Ethereum Testnet (Goerli)" %}
```javascript
const bloctoSDK = new BloctoSDK({
    ethereum: {
        chainId: '0x5', // 5
        rpc: 'https://rpc.ankr.com/eth_goerli',
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
| Ethereum Sepolia Testnet | 11155111 |
| Arbitrum Mainnet         | 42161    |
| Optimism Mainnet         | 10       |
| Polygon Mainnet          | 137      |
| Polygon Amoy Testnet   | 80002    |
| BSC Mainnet              | 56       |
| BSC Chapel Testnet       | 97       |
| Avalanche Mainnet        | 43114    |
| Avalanche Fuji Testnet   | 43113    |
| Base Mainnet             | 8453     |
| Zora Mainnet             | 7777777  |
| Scroll Mainnet           | 534352   |
| Scroll Sepolia Testnet   | 534351   |

***
