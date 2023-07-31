---
description: Use BloctoConnector to Integrate with Web3modal easily.
---

# Integrate with Web3Modal

Web3Modal is an elegantly simple yet powerful library that helps you manage your multi-chain wallet connection flows, all in one place. We provide a `BloctoConnector` for you to Integrate with `Web3Modal` easily.

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @blocto/wagmi-connector@^1.1.0
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/wagmi-connector@^1.1.0
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @blocto/wagmi-connector@^1.1.0
```
{% endtab %}
{% endtabs %}

### Step 1 - Configure `CreateConfig` with `BloctoConnector`

#### Arbitrum RPC demo

```typescript
import { w3mConnectors } from "@web3modal/ethereum";
import { createConfig } from "wagmi";
import { BloctoConnector } from '@blocto/wagmi-connector'

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

<table><thead><tr><th width="211">Paramter</th><th width="100">Type</th><th width="321">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>chains</code></td><td>Chain[]</td><td>Connector supports Chains</td><td><strong>YES</strong></td></tr><tr><td><code>options.appId</code></td><td>String</td><td>Blocto dApp ID</td><td><strong>NO</strong></td></tr><tr><td><code>options.chainId</code> (Deprecated)</td><td>Number</td><td><p>Use <code>Web3Modal.defaultChain</code> instead</p><p>EVM chain ID to connect to</p><p>Reference: <a href="https://chainid.network/">EVM Networks</a></p></td><td><strong>NO</strong></td></tr><tr><td><code>options.rpc</code></td><td>String</td><td>JSON RPC endpoint</td><td><strong>NO</strong></td></tr></tbody></table>

### Step 2 - Configure `<Web3Modal />`

```tsx
import { arbitrum } from "wagmi/chains";
import { Web3Modal } from "@web3modal/react";
import { WagmiConfig } from "wagmi";
import { BloctoWeb3ModalConfig } from '@blocto/wagmi-connector'

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

{% embed url="https://codesandbox.io/s/blocto-web3modal-integrate-sczssb" %}
