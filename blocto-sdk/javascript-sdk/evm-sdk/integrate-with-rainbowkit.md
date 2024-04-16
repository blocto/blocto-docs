---
description: You can easily add Blocto wallet to RainbowKit
---

# Integrate with RainbowKit

RainbowKit provides a fast, easy and highly customizable way for developers to add a great wallet experience to their application. It handle the hard stuff so developers and teams can focus on building amazing products and communities for their users.

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

{% hint style="warning" %}
If you wish to use autoconnect, ensure that your version of @blocto/rainbowkit-connector is 0.2.4 or higher.
{% endhint %}

### Installation

Install from npm/yarn/pnpm

RainbowKit and its peer dependencies, [wagmi](https://wagmi.sh/react/getting-started), [viem](https://viem.sh/) and [@tanstack/react-query](https://tanstack.com/query/v5).

{% tabs %}
{% tab title="npm" %}
```bash
npm install @rainbow-me/rainbowkit wagmi viem@2.x @tanstack/react-query @blocto/rainbowkit-connector
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @rainbow-me/rainbowkit wagmi viem@2.x @tanstack/react-query @blocto/rainbowkit-connector
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @rainbow-me/rainbowkit wagmi viem@2.x @tanstack/react-query @blocto/rainbowkit-connector
```
{% endtab %}
{% endtabs %}

### Step 1 - Import Rainbowkit

Import RainbowKit, wagmi, TanStack and bloctoWallet.

```javascript
import '@rainbow-me/rainbowkit/styles.css';
import {
  connectorsForWallets,
  RainbowKitProvider,
} from '@rainbow-me/rainbowkit';
import { WagmiProvider } from 'wagmi';
import { bscTestnet, blastSepolia } from "wagmi/chains";
import {
  QueryClientProvider,
  QueryClient,
} from "@tanstack/react-query";
import {
  metaMaskWallet,
  rainbowWallet,
  coinbaseWallet,
} from "@rainbow-me/rainbowkit/wallets";
import { bloctoWallet } from '@blocto/rainbowkit-connector';
```

### Step 2 - Configure

Configure your desired chains and generate the required connectors. You will also need to setup a wagmi client.
If your dApp uses server side rendering (SSR) make sure to set `ssr` to `true`.

{% hint style="warning" %}
Note: Note: Every dApp that relies on WalletConnect now needs to obtain a projectId from [WalletConnect Cloud](https://cloud.walletconnect.com/sign-in). This is absolutely free and only takes a few minutes.
{% endhint %}

```javascript
const projectId = "YOUR_PROJECT_ID";

const appName = "RainbowKit demo";

const connectors = connectorsForWallets(
  [
    {
      groupName: "Popular",
      wallets: [
        rainbowWallet,
        metaMaskWallet,
        bloctoWallet(),
        coinbaseWallet,
      ],
    },
  ],
  {
    projectId,
    appName,
  }
);

const config = createConfig({
  connectors: [...connectors],
  chains: [bscTestnet, blastSepolia],
  transports: {
    [bscTestnet.id]: http(),
    [blastSepolia.id]: http(),
  },
  multiInjectedProviderDiscovery: false,
  ssr: true, // If your dApp uses server side rendering (SSR)
});
```

[**Read more about configuring chains & providers with wagmi.**](https://wagmi.sh/react/providers/configuring-chains)

### Step 3 - Wrap providers

Wrap your application with RainbowKitProvider, WagmiProvider, and QueryClientProvider.

```javascript
const queryClient = new QueryClient();

const App = () => {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <RainbowKitProvider>
          {/* Your App */}
        </RainbowKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
};
```

### Step 4 - Add the connect button

Then, in your app, import and render the `ConnectButton` component.

```javascript
import { ConnectButton } from '@rainbow-me/rainbowkit';
export const YourApp = () => {
  return <ConnectButton />;
};
```

**RainbowKit will now handle your user's wallet selection, display wallet/transaction information and handle network/wallet switching.**

Now you can easily use `Rainbowkit` connect `BloctoWallet`

To see more configurations, please check out

* [Rainbowkit](https://www.rainbowkit.com/docs/installation)
* [Wagmi](https://wagmi.sh/react/getting-started)

## Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk-examples/tree/main/src/adapter/evm/with-rainbowkit" %}
