---
description: Use BloctoConnector to Integrate with ConnectKit easily.
---

# Integrate with ConnectKit

ConnectKit is a powerful React component library for connecting a wallet to your dApp. It supports the most popular connectors and chains, and provides a beautiful, seamless experience.

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @blocto/connectkit-connector connectkit
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/connectkit-connector connectkit
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @blocto/connectkit-connector connectkit
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**The following process requires the use of ConnectKit version >= 1.5.3**. Please ensure that you have the corresponding version installed.
{% endhint %}

### Step 1 - Configure `CreateConfig` with `BloctoConnector`

#### Arbitrum RPC demo

```typescript
//wagmi.ts
import { getDefaultConnectors } from 'connectkit'
import { configureChains, createConfig } from 'wagmi'
import { arbitrumGoerli } from 'wagmi/chains'
import { BloctoConnector } from '@blocto/connectkit-connector';
import { publicProvider } from 'wagmi/providers/public'

const { chains, publicClient, webSocketPublicClient } = configureChains(
  [arbitrumGoerli],
  [publicProvider()],
)

const walletConnectProjectId = 'YOUR_WALLETCONNECT_PROJECT_ID'

export const config = createConfig({
  connectors: [
    new BloctoConnector({ chains }),
    ...getDefaultConnectors({ chains, app: { name: 'YOUR_DAPP_NAME' }, walletConnectProjectId }),
  ],
  publicClient,
  webSocketPublicClient,
})
```

#### `BloctoConnector` parameters

<table><thead><tr><th width="211">Paramter</th><th width="100">Type</th><th width="318">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>chains</code></td><td>Chain[]</td><td>Connector supports Chains</td><td><strong>YES</strong></td></tr><tr><td><code>options.appId</code></td><td>String</td><td>Blocto dApp ID</td><td><strong>NO</strong></td></tr></tbody></table>

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

### Step 2 - Configure Blocto to supportedConnectors

```tsx
// main.tsx
import { ConnectKitProvider, supportedConnectors } from 'connectkit'
import * as React from 'react'
import * as ReactDOM from 'react-dom/client'
import { WagmiConfig } from 'wagmi'

import { App } from './App'
import { config } from './wagmi'

// configure BLOCTO_CONFIG to ensure Blocto is displayed correctly on the ConnectKit UI.
import { BLOCTO_CONFIG } from '@blocto/connectkit-connector'
supportedConnectors.push(BLOCTO_CONFIG)

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <WagmiConfig config={config}>
      <ConnectKitProvider>
        <App />
      </ConnectKitProvider>
    </WagmiConfig>
  </React.StrictMode>
)
```

### Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk-examples/tree/main/src/adapter/evm/with-connectkit?file=/src/index.tsx" %}
