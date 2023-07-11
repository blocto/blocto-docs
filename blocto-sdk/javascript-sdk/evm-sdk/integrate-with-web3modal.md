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
npm i @blocto/wagmi-connector@^1.0.0
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/wagmi-connector@^1.0.0
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @blocto/wagmi-connector@^1.0.0
```
{% endtab %}
{% endtabs %}

### Step 1 - Configure `CreateConfig` with `BloctoConnector`

#### Arbitrum RPC demo

```typescript
import { BloctoConnector } from '@blocto/wagmi-connector'

// ...

const wagmiConfig = createConfig({
  autoConnect: false,
  connectors: [new BloctoConnector({
    chains,
    options: {
      chainId: arbitrum.id,
      rpc: 'REPLACE_WITH_YOUR_ARB_RPC',
    }
  }), ...w3mConnectors({ version: 1, chains, projectId })],
  publicClient,
});

// ...
```

#### `BloctoConnector` parameters

| Paramter          | Type     | Description                                                                                            | Required |
| ----------------- | -------- | ------------------------------------------------------------------------------------------------------ | -------- |
| `chains`          | Chain\[] | Connector supports Chains                                                                              | **No**   |
| `options.chainId` | Number   | <p>EVM chain ID to connect to</p><p>Reference: <a href="https://chainid.network/">EVM Networks</a></p> | **Yes**  |
| `options.rpc`     | String   | JSON RPC endpoint                                                                                      | **Yes**  |

### Step 2 - Configure `<Web3Modal />`

```tsx
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
        ethereumClient={ethereumClient}
      />
    </>
  );
}
```

### Code Sandbox Sample

{% embed url="https://codesandbox.io/s/blocto-web3modal-integrate-sczssb" %}
