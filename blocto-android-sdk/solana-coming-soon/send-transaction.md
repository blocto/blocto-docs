# Send Transaction

Once the app is connected to Blocto wallet, it can send transactions on behalf of the user, with the user's permission.

In order to send a transaction, the app must:

* Create an unsigned transaction or transactions.
* Have it be signed by the user's Blocto wallet.
* Send it with Blocto custom JSON-RPC.

For more information about the transactions on Solana, it is recommended to check out the [solana-web3.kotlin](https://github.com/portto/solana-web3.kotlin) as well as the [official Solana docs](https://docs.solana.com/developing/programming-model/transactions).

**Plain Transaction**

For plain transactions (no dApp-side signing involved), you can just create transaction with [solana-web3.kotlin](https://github.com/portto/solana-web3.kotlin) and sign-and-send the transaction with `signAndSendTransaction` method.

```
val address = "SOLANA_ADDRESS"
val transaction = Transaction()

... // transaction manipulation

BloctoSDK.solana.signAndSendTransaction(
    context = context,
    fromAddress = address,
    transaction = transaction,
    onSuccess = { txHash ->
        // transaction sent
    },
    onError = { error ->
        // handle error
    }
)
```

**Partial Sign Transaction**

For transactions involving dApp-side signing, first you need to convert the transaction to our wallet-compatible format by calling `convertToProgramWalletTransaction`, and sign the instructions with your keys, then sign-and-send the partial-signed transaction with our `signAndSendTransaction` method.

```
suspend fun convertToProgramWalletTransaction(
    address: String,
    transaction: Transaction
): Transaction = withContext(Dispatchers.Default) {
    BloctoSDK.solana.convertToProgramWalletTransaction(address, transaction)
}

lifecycleScope.launch {
    val address = "SOLANA_ADDRESS"
    val signer = KeyPair.generate()
    val transaction = Transaction()

    ... // transaction manipulation

    val convertedTransaction = convertToProgramWalletTransaction(
        address = address, 
        transaction = transaction
    )
    convertedTransaction.partialSign(signer)

    BloctoSDK.solana.signAndSendTransaction(
        context = context,
        fromAddress = address,
        transaction = convertedTransaction,
        onSuccess = { txHash ->
            // transaction sent
        },
        onError = { error ->
            // handle error
        }
    )
}
```

{% hint style="info" %}
`BloctoSDK.solana.convertToProgramWalletTransaction(...)` method need to be executed in the background thread.
{% endhint %}
