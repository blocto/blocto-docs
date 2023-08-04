---
description: you can easily connect Blocto on thirdweb
---

# Integrate with thirdweb

thirdweb is a comprehensive development framework that empowers you to seamlessly build, launch, and manage web3 applications and games across any EVM-compatible blockchain.

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @thirdweb-dev/react @thirdweb-dev/sdk
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @thirdweb-dev/react @thirdweb-dev/sdk
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @thirdweb-dev/react @thirdweb-dev/sdk
```
{% endtab %}
{% endtabs %}

### Configuration with Blocto

Import and initiate `bloctoWallet` with `<ThirdwebProvider />`.

```javascript
import { ThirdwebProvider, bloctoWallet } from "@thirdweb-dev/react";
import {
  // Supported Mainnet Chains in Blocto
  Ethereum, Polygon, Arbitrum, Optimism, Binance, Avalanche,
  // Supported Testnet Chains in Blocto
  Goerli, Mumbai, ArbitrumGoerli, OptimismGoerli, BinanceTestnet, AvalancheFuji
} from "@thirdweb-dev/chains";

// ...

// Mainnet
const BLOCTO_SUPPORTED_MAINNET_CHAIN = [Ethereum, Polygon, Arbitrum, Optimism, Binance, Avalanche];
// Testnet
const BLOCTO_SUPPORTED_TESTNET_CHAIN = [Goerli, Mumbai, ArbitrumGoerli, OptimismGoerli, BinanceTestnet, AvalancheFuji];

function MyApp({ Component, pageProps }) {
  return (
    <ThirdwebProvider
      activeChain={Mumbai}
      supportedChains={BLOCTO_SUPPORTED_TESTNET_CHAIN}
      supportedWallets={[
        bloctoWallet({ appId: 'YOUR_DAPP_ID' }),
        {/* other wallets */}
      ]}
    >
      <App />
    </ThirdwebProvider>
  );
}
```

The default supportedChains of ThirdwebProvider includes the Blocto unsupported chain. If a user switches to an unsupported chain, it will trigger a `could not switch network error`. We strongly recommend manually configuring the supportedChains based on the current environment. You can following below list to get the latest list of Blocto supported chains.

#### `bloctowallet` parameters

<table><thead><tr><th>Paramter</th><th width="112">Type</th><th width="279">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>appId</code></td><td>string</td><td>Blocto dApp ID</td><td><strong>No</strong></td></tr></tbody></table>

#### Blocto supportedChains

<table><thead><tr><th>Mainnet</th><th>Testnet</th><th data-hidden></th></tr></thead><tbody><tr><td>Ethereum</td><td>Goerli</td><td></td></tr><tr><td>Arbitrum</td><td>ArbitrumGoerli</td><td></td></tr><tr><td>Optimism</td><td>OptimismGoerli</td><td></td></tr><tr><td>Polygon</td><td>Mumbai</td><td></td></tr><tr><td>Binance</td><td>BinanceTestnet</td><td></td></tr><tr><td>Avalanche</td><td>AvalancheFuji</td><td></td></tr></tbody></table>

### Connect to Wallet

```javascript
import { ConnectWallet } from "@thirdweb-dev/react";

// ...

const Home = () => {
  return (
    <ConnectWallet />
    
    // ...
  )
}

export default Home;
```

### Sample Code



### Resources

[**thirdweb Getting Start**](https://portal.thirdweb.com/react/getting-started)

[**ThirdwebProvider**](https://portal.thirdweb.com/react/react.thirdwebprovider)
