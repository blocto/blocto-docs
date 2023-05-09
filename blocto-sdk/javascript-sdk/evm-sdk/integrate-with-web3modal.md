---
description: Use BloctoConnector to Integrate with Web3modal easily.
---

# Integrate with Web3Modal

If you are using `Web3Modal` for your project, we provide a `BloctoConnector` for you to Integrate with `Web3Modal` easily.

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @blocto/wagmi-connector
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/wagmi-connector
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @blocto/wagmi-connector
```
{% endtab %}
{% endtabs %}

### Step 1 - Configure `CreateClient` with `BloctoConnector`

#### Arbitrum RPC demo

```typescript
import { BloctoConnector } from '@blocto/wagmi-connector'

// ...

export const client = createClient({
  autoConnect: false,
  connectors: [new BloctoConnector({
    chains: [mainnet, arbitrum ],
    options: {
      chainId: '0x66eed',
      rpc: 'YOUR_ARBITRUM_RPCURL'
    }
  }),...w3mConnectors({
    chains,
    projectId: walletConnectProjectId,
    version: 1,
  })],
  // ...
})
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

function App({ Component, pageProps }: AppProps) {
  return (
    <WagmiConfig client={client}>
      <Component {...pageProps} />
      <Web3Modal
        { ...BloctoWeb3ModalConfig }
        projectId={walletConnectProjectId}
        ethereumClient={ethereumClient}
      />
    </WagmiConfig>
  )
}
```

### Code Sandbox Sample

{% embed url="https://codesandbox.io/s/blocto-web3modal-integrate-sczssb" %}
