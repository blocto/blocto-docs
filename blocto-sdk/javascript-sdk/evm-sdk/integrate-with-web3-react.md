---
description: Use blocto-connector to connect Blocto wallet SDK with web3-react easily
---

# Integrate with Web3-React

If you are using `web3-react` for your project, we provide a connector module for you to connect Blocto wallet SDK easily.

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @blocto/web3-react-connector
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/web3-react-connector
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @blocto/web3-react-connector
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
`@blocto/web3-react-connector ^v1.0.0` is now support with `@web3-react` v8. For those using web3-react v6, please check [#using-web3-react-v6](integrate-with-web3-react.md#using-web3-react-v6 "mention")
{% endhint %}

### Usage

#### Import and initiate web3-react-connector

```typescript
import { BloctoConnector } from '@blocto/web3-react-connector'
import { initializeConnector } from '@web3-react/core'
import { URLS } from '../chains'

export const [bloctoSDK, hooks] = initializeConnector<BloctoConnector>(
  (actions) =>
    new BloctoConnector({
      actions,
      options: {
        chainId: 1,
        rpc: <YOUR_RPC_URL>,
      },
    })
)
```

#### Blocto Connector parameters

| Parameter | Type   | Description                                                                                            | Required |
| --------- | ------ | ------------------------------------------------------------------------------------------------------ | -------- |
| `chainId` | Number | <p>EVM chain ID to connect to</p><p>Reference: <a href="https://chainid.network/">EVM Networks</a></p> | **Yes**  |
| `rpc`     | String | JSON RPC endpoint                                                                                      | **Yes**  |

#### Options Example

{% tabs %}
{% tab title="Ethereum Mainnet" %}
```javascript
{
   chainId: 1,
   rpc: 'https://mainnet.infura.io/v3/YOUR_INFURA_ID',
}
```
{% endtab %}

{% tab title="Ethereum Testnet (Goerli)" %}
```javascript
{
    chainId: 5,
    rpc: 'https://rpc.ankr.com/eth_goerli',
}
```
{% endtab %}

{% tab title="BSC Mainnet" %}
```javascript
{
    chainId: 56,
}
```
{% endtab %}

{% tab title="BSC Testnet (Chapel)" %}
```javascript
{
    chainId: 97,
}
```
{% endtab %}
{% endtabs %}

| Network                  | Chain ID |
| ------------------------ | -------- |
| Ethereum Mainnet         | 1        |
| Ethereum Goerli Testnet  | 5        |
| Ethereum Sepolia Testnet | 11155111 |
| Arbitrum Mainnet         | 42161    |
| Arbitrum Goerli Testnet  | 421613   |
| Optimism Mainnet         | 10       |
| Optimism Goerli Testnet  | 420      |
| Polygon Mainnet          | 137      |
| Polygon Mumbai Testnet   | 80001    |
| BSC Mainnet              | 56       |
| BSC Chapel Testnet       | 97       |
| Avalanche Mainnet        | 43114    |
| Avalanche Fuji Testnet   | 43113    |
| Base Mainnet             | 8453     |
| Base Goerli Testnet      | 84531    |
| Zora Mainnet             | 7777777  |
| Zora Goerli Testnet      | 999      |
| Scroll Mainnet           | 534352   |
| Scroll Sepolia Testnet   | 534351   |

#### Use useWeb3React

After connector is ready, now you can activate it using `useWeb3React` hook provided by `web3-react` . Now your code might look like:

```jsx
import { useState } from 'react'
import { BloctoConnector } from '@blocto/web3-react-connector'
import { initializeConnector } from '@web3-react/core'

export const [bloctoSDK, hooks] = initializeConnector<BloctoConnector>(
  (actions) =>
    new BloctoConnector({
      actions,
      options: {
        chainId: 1,
        rpc: <YOUR_RPC_URL>,
      },
    })
)

export const Wallet = () => {
  const chainId = useChainId()
  const accounts = useAccounts()
  const isActivating = useIsActivating()
  const isActive = useIsActive()
  const [error, setError] = useState(undefined)

  const onClick = () => {
    bloctoSDK
      .activate()
      .then(() => setError(undefined))
      .catch(setError)
  }

  return (
    <div>
      <div>ChainId: {chainId}</div>
      <div>Account: {accounts[0]}</div>
      {isActive ? (
        <div>✅</div>
      ) : (
        <button type="button" onClick={onClick}>
          Connect
        </button>
      )}
    </div>
  )
}

export const App = () => {
  return (<Wallet />)
}
```

For more information using `web3-react`, please check out:

* [Web3-React](https://github.com/NoahZinsmeister/web3-react)
* [Web3-React Documentation](https://github.com/NoahZinsmeister/web3-react/tree/v6/docs)

### Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk-examples/tree/main/src/adapter/evm/with-web3-react-next?file=/connectors/bloctoSdk.ts" %}

## Using web3-react v6

For those still using web3-react v6, please use `@blocto/web3-react-connector@0.5.3`instead.

{% hint style="warning" %}
Some new feature might not support old connector in the future.
{% endhint %}

{% tabs %}
{% tab title="npm" %}
```bash
npm i @blocto/web3-react-connector@0.5.3
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/web3-react-connector@0.5.3
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @blocto/web3-react-connector@0.5.3
```
{% endtab %}
{% endtabs %}

```typescript
import { BloctoConnector } from '@blocto/web3-react-connector'

const connector = new BloctoConnector({
  chainId: NETWORK_CHAIN_ID,
  rpc: NETWORK_URL
})
```
