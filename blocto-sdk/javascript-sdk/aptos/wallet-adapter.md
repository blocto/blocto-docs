# Wallet Adapter

Blocto is integrated with [aptos wallet adapter](https://github.com/aptos-labs/aptos-wallet-adapter) (Aptos Labs), it's recommended to use the adapter to integrate with Blocto along with lots of other wallets, check out their docs for more details.

### Installation

Install from npm/yarn/pnpm

{% tabs %}
{% tab title="npm" %}
```bash
npm install @aptos-labs/wallet-adapter-react @blocto/aptos-wallet-adapter-plugin
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @aptos-labs/wallet-adapter-react @blocto/aptos-wallet-adapter-plugin
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @aptos-labs/wallet-adapter-react @blocto/aptos-wallet-adapter-plugin
```
{% endtab %}
{% endtabs %}


### Step 1 - Import Adapter

Import Aptos Adapter, bloctoWallet.

```javascript
import { useWallet } from '@aptos-labs/wallet-adapter-react';

export default function Connect() {

    const { wallets, connect, connected, disconnect } = useWallet();

    const onWalletSelect = (walletName) => {
        connect(walletName);
    };

    return (
        <div>
            {
                wallets.map((wallet) => (
                    <div key={wallet.name}>
                        {
                            !connected ? (
                                <button className="btn" onClick={() => onWalletSelect(wallet.name)}>
                                    <span>Connect {wallet.name}</span>
                                </button>
                            ) : (
                                <button className="btn" onClick={() => disconnect(wallet.name)}>
                                    <span>
                                        disconnect
                                    </span>
                                </button>
                            )
                        }
                    </div>

                ))
            }
        </div>

    )
}
```

### Step 2 - Wrap providers

Wrap your application with AptosWalletAdapterProvider inject Wallet.

```javascript
import {
  AptosWalletAdapterProvider,
  NetworkName,
} from '@aptos-labs/wallet-adapter-react';
import { BloctoWallet } from '@blocto/aptos-wallet-adapter-plugin';

const wallets = [
  new BloctoWallet({
    network: NetworkName.Testnet,
    // please register your app id at https://developers.blocto.app/
    bloctoAppId: "85d8d5db-e481-44f6-9363-7f7f4809eb39"
  })
];


const App = () => {
  return (
     <AptosWalletAdapterProvider
      plugins={wallets}
      autoConnect={false}
      onError={(error) => {
            console.log('Handle Error Message', error);
        }}
      >
      <App />
    </AptosWalletAdapterProvider>
  );
};
```

## Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk/tree/main/examples/with-aptos-adapter" %}
