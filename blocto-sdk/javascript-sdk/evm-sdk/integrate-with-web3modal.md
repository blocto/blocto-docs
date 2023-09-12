---
description: Use BloctoConnector to Integrate with Web3modal easily.
---

# Integrate with Web3Modal

Web3Modal is an elegantly simple yet powerful library that helps you manage your multi-chain wallet connection flows, all in one place. We provide a `BloctoConnector` for you to Integrate with `Web3Modal` easily.

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

{% hint style="warning" %}
Beginning with `@blocto/wagmi-connector@1.2.0`, Blocto has migrated its support for web3modal from the initial `@blocto/wagmi-connector` to `@blocto/web3modal-connector`,for earlier version's developer you can check our [migration guide](https://github.com/blocto/blocto-sdk/tree/develop/adapters/wagmi-connector#migration-guide).
{% endhint %}

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @blocto/web3modal-connector
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/web3modal-connector
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @blocto/web3modal-connector
```
{% endtab %}
{% endtabs %}

### Step 1 - Configure `CreateConfig` with `BloctoConnector`

#### Arbitrum RPC demo

```typescript
import { w3mConnectors } from "@web3modal/ethereum";
import { createConfig } from "wagmi";
import { BloctoConnector } from '@blocto/web3modal-connector'

// ...

const wagmiConfig = createConfig({
  autoConnect: false,
  connectors: [
    new BloctoConnector({ chains, options: { appId: 'YOUR_DAPP_ID' } }),
    ...w3mConnectors({ chains, projectId })
  ],
  publicClient,
});

// ...
```

#### `BloctoConnector` parameters

<table><thead><tr><th width="211">Paramter</th><th width="100">Type</th><th width="318">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>chains</code></td><td>Chain[]</td><td>Connector supports Chains</td><td><strong>YES</strong></td></tr><tr><td><code>options.appId</code></td><td>String</td><td>Blocto dApp ID</td><td><strong>NO</strong></td></tr></tbody></table>

#### Blocto supportedChains

<table><thead><tr><th width="373">Mainnet</th><th>Testnet</th><th data-hidden></th></tr></thead><tbody><tr><td>Ethereum</td><td>Goerli</td><td></td></tr><tr><td>Arbitrum</td><td>ArbitrumGoerli</td><td></td></tr><tr><td>Optimism</td><td>OptimismGoerli</td><td></td></tr><tr><td>Polygon</td><td>Mumbai</td><td></td></tr><tr><td>Binance</td><td>BinanceTestnet</td><td></td></tr><tr><td>Avalanche</td><td>AvalancheFuji</td><td></td></tr></tbody></table>

### Step 2 - Configure `<Web3Modal />`

```tsx
import { arbitrum } from "wagmi/chains";
import { Web3Modal } from "@web3modal/react";
import { WagmiConfig } from "wagmi";
import { BloctoWeb3ModalConfig } from '@blocto/web3modal-connector'

// ...

export default function App({ Component, pageProps }) {
  // ...

  return (
    <>
      {ready ? (
        <WagmiConfig config={wagmiConfig}>
          <Component {...pageProps} />
        </WagmiConfig>
      ) : null}

      <Web3Modal
        { ...BloctoWeb3ModalConfig }
        projectId={projectId}
        defaultChain={arbitrum}
        ethereumClient={ethereumClient}
      />
    </>
  );
}
```

### Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk/tree/develop/examples/with-evm-web3modal?file=/src/App.tsx" %}
