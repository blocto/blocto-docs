# Wallet adapter

Blocto wallet adapter is the extension of the official [aptos wallet adapter](https://github.com/aptos-labs/aptos-wallet-adapter), which provides blocto wallet support and extra features.

### Installation

Install from npm/yarn

```bash
$ yarn add @blocto/aptos-wallet-adapter
```

### React Provider

```javascript
import React from 'react';
import {
  WalletProvider,
  BloctoWalletAdapter,
  HippoWalletAdapter,
  AptosWalletAdapter,
  HippoExtensionWalletAdapter,
  MartianWalletAdapter,
  FewchaWalletAdapter,
  PontemWalletAdapter,
  SpikaWalletAdapter,
  RiseWalletAdapter,
  FletchWalletAdapter,
} from '@blocto/aptos-wallet-adapter';

const wallets = [
  new BloctoWalletAdapter(),
  new HippoWalletAdapter(),
  new MartianWalletAdapter(),
  new AptosWalletAdapter(),
  new FewchaWalletAdapter(),
  new HippoExtensionWalletAdapter(),
  new PontemWalletAdapter(),
  new SpikaWalletAdapter(),
  new RiseWalletAdapter(),
  new FletchWalletAdapter()
];

const App: React.FC = () => {
  return (
    <WalletProvider
      wallets={wallets}
      autoConnect={true | false} /** allow auto wallet connection or not **/
      onError={(error: Error) => {
        console.log('Handle Error Message', error);
      }}>
      {/* your website */}
    </WalletProvider>
  );
};

export default App;
```

If you want to develop on other networks with blocto wallet, simply pass in the `network` config:

```javascript
import { 
 // ...
  WalletAdapterNetwork
} from '@blocto/aptos-wallet-adapter';

const wallets = [
  new BloctoWalletAdapter({ network: WalletAdapterNetwork.Testnet }),
  // ...
];
// ...
```

### React Hook

```javascript
import { useWallet } from '@blocto/aptos-wallet-adapter';

const { connected, account, network, ...rest } = useWallet();

/*
  ** Properties available: **

  wallets: Wallet[]; - Array of wallets
  wallet: Wallet | null; - Selected wallet
  account: AccountKeys | null; - Wallet info: address, 
  network: NetworkInfo - { name, chainId, api }
  publicKey, authKey
  connected: boolean; - check the website is connected yet
  connect(walletName: string): Promise<void>; - trigger connect popup
  disconnect(): Promise<void>; - trigger disconnect action
  signAndSubmitTransaction(
    transaction: TransactionPayload
  ): Promise<PendingTransaction>; - function to sign and submit the transaction to chain
*/
```

### Connect wallet

```javascript
import { AptosWalletName, useWallet } from "@blocto/aptos-wallet-adapter"

...

const { connect, disconnect, connected, select } = useWallet();

/** If auto-connect is not enabled, you will require to do the connect() manually **/
useEffect(() => {
  if (!autoConnect && currentWallet?.adapter) {
    connect();
  }
}, [autoConnect, currentWallet, connect]);
/** this is only required if you do not want auto connect wallet **/

if (!connected) {
  return (
    <button
      onClick={() => {
        select(); // E.g. connecting to the Aptos official wallet (Breaking Change)
      }}
    >
      Connect
    </button>
  );
} else {
  return (
    <button
      onClick={() => {
        disconnect();
      }}
    >
      Disconnect
    </button>
  );
}
```

### Submit transaction

```javascript
const { signAndSubmitTransaction } = useWallet();
// transfer 1000 Octas to 0x123
const transaction = {
  arguments: ['0x123', '1000'],
  function: '0x1::coin::transfer',
  type: 'entry_function_payload',
  type_arguments: ['0x1::aptos_coin::AptosCoin'],
}
... 
// sign and submit the transaction & get the tx hash
const { hash } await signAndSubmitTransaction(transaction)
```

There's a complete example [here](https://github.com/portto/aptos-wallet-adapter/tree/main/packages/wallet-tester)
