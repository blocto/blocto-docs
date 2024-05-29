---
description: You can easily use Blocto on Wagmi
---

# Integrate with Wagmi

Wagmi is a collection of React Hooks containing everything you need to start working with Ethereum. Wagmi makes it easy to "Connect Wallet," display ENS and balance information, sign messages, interact with contracts, and much more — all with caching, request deduplication, and persistence.

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @blocto/wagmi-connector@^2 wagmi viem@2.x @tanstack/react-query
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/wagmi-connector@^2 wagmi viem@2.x @tanstack/react-query
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @blocto/wagmi-connector@^2 wagmi viem@2.x @tanstack/react-query
```
{% endtab %}
{% endtabs %}

### Create config with Blocto

```javascript
import { blocto } from '@blocto/wagmi-connector'
import { http, createConfig } from 'wagmi'
import { polygonAmoy, mainnet, sepolia } from 'wagmi/chains'

export const config = createConfig({
  chains: [polygonAmoy, mainnet, sepolia],
  connectors: [blocto({ appId: 'REPLACE_WITH_YOUR_DAPP_ID' })],
  transports: {
    [polygonAmoy.id]: http(),
    [mainnet.id]: http(),
    [sepolia.id]: http(),
  },
})
```

#### `blocto` parameters

<table><thead><tr><th width="211">Paramter</th><th width="100">Type</th><th width="266">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>appId</code></td><td>String</td><td>Blocto dApp ID</td><td><strong>NO</strong></td></tr></tbody></table>

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
      <td>Amoy</td>
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

### Wrap app with `<WagmiConfig />` and `<QueryClientProvider />`

```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query' 
import { WagmiProvider } from 'wagmi'
import { config } from './config'

const queryClient = new QueryClient() 

function App() {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}> 
        {/** ... */} 
      </QueryClientProvider> 
    </WagmiProvider>
  )
}
```

### Use Blocto with Wagmi

```tsx
import { useAccount, useEnsName } from 'wagmi'

function Profile() {
  const { address } = useAccount()
  const { data, error, status } = useEnsName({ address })
  if (status === 'pending') return <div>Loading ENS name</div>
  if (status === 'error')
    return <div>Error fetching ENS name: {error.message}</div>
  return <div>ENS name: {data}</div>
}
```

### Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk-examples/tree/main/src/adapter/evm/with-wagmi" %}

### Resources

[**Getting Started**](https://wagmi.sh/react/getting-started)

[**Wagmi Docs**](https://wagmi.sh/)
