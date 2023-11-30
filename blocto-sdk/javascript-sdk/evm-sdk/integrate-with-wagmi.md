---
description: You can easily use Blocto on wagmi
---

# Integrate with wagmi

wagmi is a collection of React Hooks containing everything you need to start working with Ethereum. wagmi makes it easy to "Connect Wallet," display ENS and balance information, sign messages, interact with contracts, and much more — all with caching, request deduplication, and persistence.



Blocto currently supported various wallet connect solutions based on wagmi,

* [Integrate with RainbowKit](integrate-with-rainbowkit.md)
* [Integrate with web3modal](integrate-with-web3modal.md)
* [Integrate with thirdweb](integrate-with-thirdweb.md)

these solutions provide an amazing user experience. before you begin reading, you can take a look at the links above.

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @blocto/wagmi-connector wagmi viem
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/wagmi-connector wagmi viem
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @blocto/wagmi-connector wagmi viem
```
{% endtab %}
{% endtabs %}

### Configure chains

```javascript
import { configureChains, mainnet } from 'wagmi'
import { mainnet, arbitrum, optimism, polygon, bsc, avalanche } from 'wagmi/chains'
import { goerli, arbitrumGoerli, optimismGoerli, polygonMumbai, bscTestnet, avalancheFuji  } from 'wagmi/chains'
import { publicProvider } from 'wagmi/providers/public'

// Mainnet
const BLOCTO_SUPPORTED_MAINNET_CHAIN = [mainnet, arbitrum, optimism, polygon, bsc, avalanche];
// Testnet
const BLOCTO_SUPPORTED_TESTNET_CHAIN = [goerli, arbitrumGoerli, optimismGoerli, polygonMumbai, bscTestnet, avalancheFuji];

const { chains, publicClient, webSocketPublicClient } = configureChains(
  BLOCTO_SUPPORTED_TESTNET_CHAIN,
  [publicProvider()],
)
```

**Using `publicProvider` in a production environment is not recommended**. It's advisable to use `alchemyProvider` or `infuraProvider` as alternatives for the production environment's provider.

#### Blocto supportedChains

<table><thead><tr><th width="373">Mainnet</th><th>Testnet</th><th data-hidden></th></tr></thead><tbody><tr><td>Ethereum</td><td>Goerli</td><td></td></tr><tr><td>Arbitrum</td><td>ArbitrumGoerli</td><td></td></tr><tr><td>Optimism</td><td>OptimismGoerli</td><td></td></tr><tr><td>Polygon</td><td>Mumbai</td><td></td></tr><tr><td>Binance</td><td>BinanceTestnet</td><td></td></tr><tr><td>Avalanche</td><td>AvalancheFuji</td><td></td></tr></tbody></table>

### Create config with Blocto

```javascript
import { BloctoConnector } from '@blocto/wagmi-connector'

const config = createConfig({
  autoConnect: true,
  connectors: [new BloctoConnector({ chains, options: { appId: 'REPLACE_WITH_YOUR_DAPP_ID' } })]
  publicClient,
  webSocketPublicClient,
})
```

#### `BloctoConnector` parameters

<table><thead><tr><th width="211">Paramter</th><th width="100">Type</th><th width="318">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>chains</code></td><td>Chain[]</td><td>Connector supports Chains</td><td><strong>YES</strong></td></tr><tr><td><code>options.appId</code></td><td>String</td><td>Blocto dApp ID</td><td><strong>NO</strong></td></tr></tbody></table>

### Wrap app with `<WagmiConfig />`

```jsx
function App() {
  return (
    <WagmiConfig config={config}>
      <YourRoutes />
    </WagmiConfig>
  )
}
```

### Use Blocto with wagmi

```tsx
import { useAccount, useConnect, useEnsName } from 'wagmi'

function Profile() {
  const { address, isConnected } = useAccount()
  const { data: ensName } = useEnsName({ address })
  const { connect, connectors } = useConnect()
  const [blocto] = connectors
  if (isConnected) return <div>Connected to {ensName ?? address}</div>
  return <button onClick={() => connect({ connector: blocto })}>Connect Wallet</button>
}

```

### Sample Code

{% embed url="https://codesandbox.io/p/sandbox/github/blocto/blocto-sdk-examples/tree/main/with-evm-wagmi?file=/src/wagmi.ts" %}

### Resources

[**Getting Started**](https://wagmi.sh/react/getting-started)

[**Example**](https://wagmi.sh/examples/connect-wallet)

