---
description: A thin JSON-RPC wrapper for interacting with chains and Blocto wallet.
---

# Provider

Blocto SDK comes with an [EIP-1193](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1193.md) compatible provider, you can use it to interact with EVM-compatible chains.

{% hint style="info" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @blocto/sdk
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/sdk
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @blocto/sdk
```
{% endtab %}
{% endtabs %}

... or via CDN

```javascript
<script src="https://unpkg.com/@blocto/sdk" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
```

### **Usage**

#### Initiate Blocto SDK

```javascript
import BloctoSDK from '@blocto/sdk'

const bloctoSDK = new BloctoSDK({
    ethereum: {
        // (required) chainId to be used
        chainId: '0x1', 
        // (required) JSON RPC endpoint
        rpc: 'https://mainnet.infura.io/v3/YOUR_INFURA_ID',
    },
    
    // (optional) Blocto app ID
    appId: 'YOUR_BLOCTO_APP_ID',
});
```

#### Blocto SDK Parameters

<table><thead><tr><th width="212">Parameter</th><th width="138">Type</th><th>Description</th><th>Required</th></tr></thead><tbody><tr><td><code>ethereum.chainId</code></td><td>String (hex)<br>Number</td><td><p>EVM chain ID to connect to</p><p>Reference: <a href="https://chainid.network/">EVM Networks</a></p></td><td><strong>Yes</strong></td></tr><tr><td><code>ethereum.rpc</code></td><td>String</td><td>JSON RPC endpoint</td><td><strong>Yes</strong></td></tr><tr><td><code>appId</code></td><td>String</td><td>Blocto dApp ID</td><td><strong>No</strong></td></tr></tbody></table>

#### Examples

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
        chainId: '0x5', // 4
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

| Network                 | Chain ID |
| ----------------------- | -------- |
| Ethereum Mainnet        | 1        |
| Ethereum Goerli Testnet | 5        |
| Arbitrum Mainnet        | 42161    |
| Arbitrum Goerli Testnet | 421613   |
| Optimism Mainnet        | 10       |
| Optimism Goerli Testnet | 420      |
| Polygon Mainnet         | 137      |
| Polygon Mumbai Testnet  | 80001    |
| BSC Mainnet             | 56       |
| BSC Chapel Testnet      | 97       |
| Avalanche Mainnet       | 43114    |
| Avalanche Fuji Testnet  | 43113    |

#### **Connect to Blocto wallet**

Once the connection request is fired, there would be a prompt modal to guide user to register/login to Blocto wallet

```javascript
// EIP-1193 way (recommended)
const accounts = bloctoSDK.ethereum.request({ method: 'eth_requestAccounts' })

// Alternative: EIP-1102 way
// CAVEATS! it's deprecated and may be removed from future version
const accounts = await bloctoSDK.ethereum.enable()
```

After connected with Blocto wallet, you can start to send JSON-RPC request with blocto provider

```javascript
// sign a message
bloctoSDK.ethereum.request({
  method: 'eth_sign', 
  params: ["0xyourethaddress", "0x48656c6c6f20776f726c64"]
})
```
