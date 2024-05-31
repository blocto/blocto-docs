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

After SDK `0.9.0`, you can load all evm-compatiable chains once when initiating.

<table><thead><tr><th width="180">Parameter</th><th width="288">Type</th><th width="195">Description</th><th>Required</th></tr></thead><tbody><tr><td>defaultChainId</td><td>string (hex)</td><td>Hexadecimal chainId</td><td>Yes</td></tr><tr><td>switchableChains</td><td><p>Array</p><pre class="language-typescript"><code class="lang-typescript">AddEthereumChainParameter {
  chainId: string;
  rpcUrls: string[];
}
</code></pre></td><td>Array of AddEthereumChainParameter</td><td>Yes</td></tr></tbody></table>

```javascript
const bloctoSDK = new BloctoSDK({
  ethereum: {
    defaultChainId: '0x1',
    switchableChains: [
      {
        chainId: '0x1',
        rpcUrls: ['https://mainnet.infura.io/v3/...'],
      },
      {
        chainId: '0xa4b1',
        rpcUrls: ['https://arb1.arbitrum.io/rpc'],
      },
      ...
    ],
  },
  
  // (optional) Blocto app ID
  appId: 'YOUR_BLOCTO_APP_ID',
})
```

#### Initiate Blocto SDK before v0.9.0

Before `0.9.0` , you can only initiate with one evm-compatiable chain.

<table><thead><tr><th width="212">Parameter</th><th width="138">Type</th><th>Description</th><th>Required</th></tr></thead><tbody><tr><td><code>ethereum.chainId</code></td><td>String (hex)<br>Number</td><td><p>EVM chain ID to connect to</p><p>Reference: <a href="https://chainid.network/">EVM Networks</a></p></td><td><strong>Yes</strong></td></tr><tr><td><code>ethereum.rpc</code></td><td>String</td><td>JSON RPC endpoint</td><td><strong>Yes</strong></td></tr><tr><td><code>appId</code></td><td>String</td><td>Blocto dApp ID</td><td><strong>No</strong></td></tr></tbody></table>

```java
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

### Support Chains

| Network                  | Chain ID |
| ------------------------ | -------- |
| Ethereum Mainnet         | 1        |
| Ethereum Sepolia Testnet | 11155111 |
| Arbitrum Mainnet         | 42161    |
| Arbitrum Sepolia         | 421614    |
| Optimism Mainnet         | 10       |
| Optimism Sepolia         | 11155420       |
| Polygon Mainnet          | 137      |
| Polygon Amoy Testnet   | 80002    |
| BSC Mainnet              | 56       |
| BSC Chapel Testnet       | 97       |
| Avalanche Mainnet        | 43114    |
| Avalanche Fuji Testnet   | 43113    |
| Base Mainnet             | 8453     |
| Base Sepolia             | 84532     |
| Zora Mainnet             | 7777777  |
| Zora Sepolia             | 999999999  |
| Scroll Mainnet           | 534352   |
| Scroll Sepolia Testnet   | 534351   |

### **Connect to Blocto wallet**

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
