---
description: You can easily add Blocto wallet to RainbowKit
---

# RainbowKit

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @rainbow-me/rainbowkit wagmi ethers@^5 @blocto/rainbowkit-connector
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @rainbow-me/rainbowkit wagmi ethers@^5 @blocto/rainbowkit-connector
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @rainbow-me/rainbowkit wagmi ethers@^5 @blocto/rainbowkit-connector
```
{% endtab %}

{% endtabs %}
### Step 1 - Import Rainbowkit

#### Import RainbowKit, wagmi, and bloctoWallet.

```javascript
import '@rainbow-me/rainbowkit/styles.css';
import {
  getDefaultWallets,
  connectorsForWallets,
  RainbowKitProvider,
} from '@rainbow-me/rainbowkit';
import { configureChains, createClient, WagmiConfig } from 'wagmi';
import { mainnet, polygon, optimism, arbitrum } from 'wagmi/chains';
import { alchemyProvider } from 'wagmi/providers/alchemy';
import { publicProvider } from 'wagmi/providers/public';
import { bloctoWallet } from '@blocto/rainbowkit-connector';
```

### Step 2 - Configure 

#### Configure your desired chains and generate the required connectors. You will also need to setup a wagmi client.
{% hint style="warning" %}
Note: Note: Every dApp that relies on WalletConnect now needs to obtain a projectId from [WalletConnect Cloud](https://cloud.walletconnect.com/sign-in). This is absolutely free and only takes a few minutes.
{% endhint %}


```javascript
const { chains, provider } = configureChains(
  [mainnet, polygon, optimism, arbitrum], 
  [
    alchemyProvider({ apiKey: process.env.ALCHEMY_ID }),
    publicProvider()
  ]
);

const { wallets } = getDefaultWallets({
  appName: 'My RainbowKit App',
  projectId: 'YOUR_PROJECT_ID',
  chains
});

const connectors = connectorsForWallets([
  ...wallets,
  {
    groupName: 'Other',
    wallets: [
      bloctoWallet({ chains }), // add BloctoWallet
    ],
  },
]);

const wagmiClient = createClient({
  autoConnect: true,
  connectors,
  provider
})
```

##### [Read more about configuring chains & providers with wagmi.](https://wagmi.sh/react/providers/configuring-chains)

### Step 3 - Wrap providers

#### Wrap your application with RainbowKitProvider and WagmiConfig.

```javascript
const App = () => {
  return (
    <WagmiConfig client={wagmiClient}>
      <RainbowKitProvider chains={chains}>
        <YourApp />
      </RainbowKitProvider>
    </WagmiConfig>
  );
};
```

### Step 4 - Add the connect button

#### Then, in your app, import and render the ```ConnectButton``` component.

```javascript
import { ConnectButton } from '@rainbow-me/rainbowkit';
export const YourApp = () => {
  return <ConnectButton />;
};
```
##### RainbowKit will now handle your user's wallet selection, display wallet/transaction information and handle network/wallet switching.

Now you can easily use `Rainbowkit` connect `BloctoWallet` 

To see more configurations, please check out

* [Rainbowkit](https://www.rainbowkit.com/docs/installation#wrap-providers)
* [Wagmi](https://wagmi.sh/react/getting-started)

### Code Sandbox Sample

{% embed url="https://codesandbox.io/s/evm-rainbowkit-dncj9j" %}
