---
description: You can easily use Blocto on Wagmi
---

# Integrate with Wagmi v1

Wagmi is a collection of React Hooks containing everything you need to start working with Ethereum. Wagmi makes it easy to "Connect Wallet," display ENS and balance information, sign messages, interact with contracts, and much more — all with caching, request deduplication, and persistence.

Blocto currently supported various wallet connect solutions based on Wagmi,

* [Integrate with RainbowKit(v1)](integrate-with-rainbowkit-v1.md)
* [Integrate with web3modal](integrate-with-web3modal.md)
* [Integrate with thirdweb](integrate-with-thirdweb.md)

these solutions provide an amazing user experience. before you begin reading, you can take a look at the links above.

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @blocto/wagmi-connector@^1.3.1 wagmi@^1.4.12 viem@~1.0.0
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/wagmi-connector@^1.3.1 wagmi@^1.4.12 viem@~1.0.0
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @blocto/wagmi-connector@^1.3.1 wagmi@^1.4.12 viem@~1.0.0
```
{% endtab %}
{% endtabs %}

### Configure chains

```javascript
import { configureChains, mainnet } from 'wagmi'
import { mainnet, arbitrum, optimism, polygon, bsc, avalanche } from 'wagmi/chains'
import { bscTestnet, sepolia  } from 'wagmi/chains'
import { publicProvider } from 'wagmi/providers/public'

// Mainnet
const BLOCTO_SUPPORTED_MAINNET_CHAIN = [mainnet, arbitrum, optimism, polygon, bsc, avalanche];
// Testnet
const BLOCTO_SUPPORTED_TESTNET_CHAIN = [bscTestnet, sepolia];

const { chains, publicClient, webSocketPublicClient } = configureChains(
  BLOCTO_SUPPORTED_TESTNET_CHAIN,
  [publicProvider()],
)
```

**Using `publicProvider` in a production environment is not recommended**. It's advisable to use `alchemyProvider` or `infuraProvider` as alternatives for the production environment's provider.

#### Blocto supportedChains

<table>
  <thead>
    <tr>
      <th width="373">Mainnet</th>
      <th>Testnet</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>Ethereum</td>
      <td>Sepolia</td>
    </tr>
    <tr>
      <td>Arbitrum</td>
      <td>ArbitrumSepolia</td>
    </tr>
    <tr>
      <td>Optimism</td>
      <td>OptimismSepolia</td>
    </tr>
    <tr>
      <td>Polygon</td>
      <td>X</td>
    </tr>
    <tr>
      <td>Binance</td>
      <td>BinanceTestnet</td>
    </tr>
    <tr>
      <td>Avalanche</td>
      <td>AvalancheFuji</td>
    </tr>
    <tr>
      <td>Base</td>
      <td>BaseSepolia</td>
    </tr>
    <tr>
      <td>Zora</td>
      <td>ZoraTestnet</td>
    </tr>
    <tr>
      <td>Scroll</td>
      <td>ScrollSepolia</td>
    </tr>
    </tbody>
  </table>

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

### Use Blocto with Wagmi

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

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk-examples/tree/main/src/adapter/evm/with-wagmi-v1?file=/src/wagmi.ts" %}

### Resources

[**Getting Started**](https://wagmi.sh/react/getting-started)

[**Example**](https://wagmi.sh/examples/connect-wallet)
