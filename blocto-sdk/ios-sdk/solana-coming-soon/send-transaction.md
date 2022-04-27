# Send Transaction

Once your app is connected to Blocto wallet, it can send transactions on behalf of the user, with the user's permission.

In order to send a transaction, the app must:

* Create an unsigned transaction or transactions using Solana web3.
* Have it be signed by the user's Blocto wallet.
* Sign it and send it with Blocto SDKfix.

For more information about the transactions on Solana, it is recommended to check out the [SolanaWeb3](https://github.com/portto/solana-web3.swift) as well as the [official Solana docs](https://docs.solana.com/developing/programming-model/transactions).

**Plain Transaction**

For plain transactions (no dApp-side signing involved), you can just create transaction with [SolanaWeb3](https://github.com/portto/solana-web3.swift) and sign-and-send the transaction with `signAndSendTransaction` method.

```
val userWalletAddress = "SOLANA_ADDRESS"
val transaction = Transaction()

... // transaction manipulation

BloctoSDK.shared.solana.signAndSendTransaction(
    from: userWalletAddress,
    transaction: transaction) { [weak self] result in
        guard let self = self else { return }
        self.resetSetValueStatus()
        switch result {
        case let .success(txHash):
            // handle txHash here
        case let .failure(error):
            // handle error here
        }
    }
```

**Partial Sign Transaction**

For transactions involving dApp-side signing, first you need to convert the transaction to our wallet-compatible format by calling `convertToProgramWalletTransaction`, and sign the instructions with your keys, then sign-and-send the partial-signed transaction with our `signAndSendTransaction` method.

```
val userWalletAddress = "SOLANA_ADDRESS"
val transaction = Transaction()

... // transaction manipulation

BloctoSDK.shared.solana.convertToProgramWalletTransaction(
    transaction,
    solanaAddress: userWalletAddress) { [weak self] result in
        guard let self = self else { return }
        switch result {
        case let .success(transaction):
            do {
                var newTransaction = transaction
                // partial sign after transaction converted.
                try newTransaction.partialSign(signers: [newAccount])
                
                BloctoSDK.shared.solana.signAndSendTransaction(
                    from: userWalletAddress,
                    transaction: newTransaction) { [weak self] result in
                        switch result {
                        case let .success(txHash):
                            // handle txHash here
                        case let .failure(error):
                            // handle error here
                        }
                    }
            } catch {
                // handle error here
            }
        case let .failure(error):
            // handle error here
        }
    }
```

{% hint style="warning" %}
Make sure you [initialize Blocto SDK](https://docs.blocto.app/blocto-sdk/ios-sdk/solana-coming-soon/getting-started#initialize-blocto-sdk) first
{% endhint %}
