---
description: You can easily add Blocto wallet to Web3-Onboard
---

# Integrate with Web3-Onboard

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

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
```

### Step 2 - Configure and add button

Configure your desired chains and generate the required connectors.

```javascript
const MAINNET_RPC_URL = "https://mainnet.infura.io/v3/<INFURA_KEY>";

const blocto = bloctoModule();

const onboard = Onboard({
  wallets: [blocto],
  chains: [
    {
      id: "0x1",
      token: "ETH",
      label: "Ethereum Mainnet",
      rpcUrl: MAINNET_RPC_URL,
    },
  ],
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

Now you can easily use `Web3-Onboard` connect `BloctoWallet`

To see more configurations, please check out

- [Web3-Onboard](https://onboard.blocknative.com/)


### Code Sandbox Sample (React)

{% embed url="https://codesandbox.io/s/evm-web3-onbard-4u055j" %}
