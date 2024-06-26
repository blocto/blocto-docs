---
description: You can easily add Blocto wallet to Web3-Onboard
---

# Integrate with Web3-Onboard

Web3-Onboard is an open-source, framework-agnostic JavaScript library made by Blocknative to onboard users to web3 apps. Help your users transact with ease by enabling wallet connection, real-time transaction states, and more.

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

## VanillaJS

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @web3-onboard/core @web3-onboard/blocto ethers
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @web3-onboard/core @web3-onboard/blocto ethers
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @web3-onboard/core @web3-onboard/blocto ethers
```
{% endtab %}
{% endtabs %}

### Step 1 - Import Web3-Onboard

Import Web3-Onboard, ethers, and bloctoWallet.

```javascript
import Onboard from "@web3-onboard/core";
import bloctoModule from "@web3-onboard/blocto";
import { ethers } from "ethers";
import { bscTestnet, sepolia } from 'wagmi/chains'

```

### Step 2 - Configure and add button

Configure your desired chains and generate the required connectors.

```javascript

const blocto = bloctoModule();

const onboard = Onboard({
  wallets: [blocto],
  chains: [bscTestnet, sepolia],
});

// connect wallet
const connectWallet = async () => {
  await onboard.connectWallet();
};

const sendTx = async () => {
  // create an ethers provider with the last connected wallet provider
  // if using ethers v6 this is:
  // ethersProvider = new ethers.BrowserProvider(wallet.provider, 'any')
  const ethersProvider = new ethers.providers.Web3Provider(
    wallets[0].provider,
    "any"
  );
  const signer = ethersProvider.getSigner();
  // send a transaction with the ethers provider
  const txn = await signer.sendTransaction({
    to: "0x",
    value: 100000000000000,
  });
  const receipt = await txn.wait();
  console.log(receipt);
};

const App = () => {
  return (
    <button onClick={connectWallet}>
        connect wallet
    </button>
  );
};
```

Now you can easily use `Web3-Onboard` connect `BloctoWallet`

To see more configurations, please check out

* [Web3-Onboard](https://onboard.blocknative.com/)

### Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk-examples/tree/main/src/adapter/evm/with-web3onboard-vanilla?file=/service.js" %}

## React.js

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm i @web3-onboard/react @web3-onboard/blocto ethers
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @web3-onboard/react @web3-onboard/blocto ethers
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @web3-onboard/react @web3-onboard/blocto ethers
```
{% endtab %}
{% endtabs %}

### Step 1 - Import Web3-Onboard

Import Web3-Onboard, ethers, and bloctoWallet.

```javascript
import injectedModule from "@web3-onboard/injected-wallets";
import { init, useConnectWallet } from "@web3-onboard/react";
import bloctoModule from "@web3-onboard/blocto";
import { ethers } from "ethers";
```

### Step 2 - Configure

Configure your desired chains and generate the required connectors.

```javascript
const bscTestnet = {
  id: "0x61",
  token: "BNB",
  label: "Polygon",
  rpcUrl: "https://data-seed-prebsc-1-s1.binance.org:8545/"
};

const chains = [polygonTestnet];
const wallets = [bloctoModule()];

const web3Onboard = init({
  wallets,
  chains,
  appMetadata: {
    name: "Web3-Onboard Demo",
    icon: "<svg>My App Icon</svg>",
    description: "A demo of Web3-Onboard."
  }
});
```

### Step 3 - Wrap providers

Wrap your application with Web3OnboardProvider.

```javascript
  <Web3OnboardProvider web3Onboard={web3Onboard}>
    <App />
  </Web3OnboardProvider>
```

### Step 4 - Inject provider and `connect wallet`

```javascript
  const [{ wallet, connecting }, connect, disconnect] = useConnectWallet();

  const ethersProviderRef = useRef();
  if (wallet && !ethersProviderRef.current) {
    // if using ethers v6 this is:
    // ethersProvider = new ethers.BrowserProvider(wallet.provider, 'any')
    ethersProviderRef.current = new ethers.providers.Web3Provider(
      wallet.provider,
      "any"
    );
  }

const connectWallet = async () => {

}

const App = () => {
  return (
      <button
        disabled={connecting}
        onClick={() => (wallet ? disconnect(wallet) : connect())}
      >
        <span>
          {connecting ? "Connecting" : wallet ? "Disconnect" : "Connect"}
        </span>
      </button>
  );
};
```

To see more configurations, please check out

* [Web3-Onboard React](https://onboard.blocknative.com/docs/modules/react)

### Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk-examples/tree/main/src/adapter/evm/with-web3onboard?file=/src/service/index.js" %}
