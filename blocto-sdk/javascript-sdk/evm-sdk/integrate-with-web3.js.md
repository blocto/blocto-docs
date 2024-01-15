---
description: Use Blocto wallet SDK with your existing ethers integration
---

# Integrate with ethers.js

You can use Blocto wallet service with your current ethers integration with only a few simple lines of code.

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i ethers @blocto/sdk
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add ethers @blocto/sdk
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add ethers @blocto/sdk
```
{% endtab %}
{% endtabs %}

### Usage

#### Initiate Blocto SDK and ethers

<pre class="language-javascript"><code class="lang-javascript">import { ethers } from "ethers";
import BloctoSDK from '@blocto/sdk'

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

// If using ethers.js v6 or after
const provider = new ethers.BrowserProvider(bloctoSDK.ethereum);
// If using ethers.js v5, this will be
<strong>// const provider = new ethers.providers.Web3Provider(bloctoSDK.ethereum);
</strong>
// Blocto SDK requires requesting permission to connect users accounts
await provider.send("eth_requestAccounts", []);

// Use getSigner to get access with write operations like send transaction
const signer = provider.getSigner()
// Send transaction
const txn = await signer.sendTransaction(transaction);
</code></pre>

Now you can then use the `provider` and `signer` object for on-chain interactions. For more documentation for what you can do with `ethers.js`, please check out

* [https://ethers.org/](https://ethers.org/)

### Example Code

{% embed url="https://codesandbox.io/p/sandbox/github/blocto/blocto-sdk-examples/tree/main/with-evm-web3onboard?file=/src/App.js:12,38" %}

***
