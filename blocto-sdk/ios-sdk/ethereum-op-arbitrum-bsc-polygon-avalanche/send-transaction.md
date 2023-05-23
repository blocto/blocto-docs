# Send Transaction

{% hint style="warning" %}
Make sure you [initialize Blocto SDK](../solana/getting-started.md) first
{% endhint %}

Once your app is connected to Blocto wallet, it can send transactions on behalf of the user, with the user's permission.

In order to send a transaction, the app must:

* Create an Ethereum transaction base on predefined type `EVMBaseTransaction`.
* Have it be signed by the user's Blocto wallet.
* Send it with Blocto custom JSON-RPC.

For more information about the transaction on EVMBase, it is recommended to check out the [Ethereum Web3](https://github.com/portto/web3.swift) as well as [official Ethereum docs](https://ethereum.org/en/developers/docs/transactions/).

**Send Transaction**

```swift
let evmBaseTransaction = EVMBaseTransaction(
    to: "0x...", // contract address that user want to interact with.
    from: "0x...", // user address.
    value: "0x64", // 100 wei hex string with 0x prefix, default is 0.
    data: functionData) // functionData stands for data in ethereum transaction, default is Empty data.
BloctoSDK.shared.evm.sendTransaction(
    blockchain: .ethereum,
    transaction: evmBaseTransaction
) { [weak self] result in
    guard let self = self else { return }
    switch result {
    case let .success(txHsh):
        // handle txHash here.
    case let .failure(error):
        // handle error here.
    }
}
```
