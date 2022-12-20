# Send Transaction

{% hint style="info" %}
Make sure you[ initialize Blocto SDK](getting-started.md) first
{% endhint %}

Once your app is connected to Blocto wallet, it can send transactions on behalf of the user, with the user's permission.

In order to send a transaction, the app must:

* Create an unsigned transaction or transactions.
* Have it be signed by the user's Blocto wallet.
* Send it with Blocto custom JSON-RPC.

For more information about the transactions on Solana, it is recommended to check out the [solnet](https://github.com/bmresearch/Solnet) as well as the [official Solana docs](https://docs.solana.com/developing/programming-model/transactions).

### **Plain Transaction**

For plain transactions (no dApp-side signing involved), you can just create transaction with [solnet](https://github.com/bmresearch/Solnet) and sign-and-send the transaction with `SignAndSendTransaction` method.

```csharp
var address = "SOLANA_ADDRESS";
var transaction = new Transaction();

... // transaction manipulation

bloctoWalletProvider.SignAndSendTransaction(address, transaction, txhash => {                                                                     
                                                                        Debug.Log($"Tx hash: {txhash}");
                                                                  }); 
```

### **Partial Sign Transaction**

For transactions involving dApp-side signing, first, you need to convert the transaction to our wallet-compatible format by calling `ConvertToProgramWalletTransaction`, and sign the instructions with your keys, then sign and send the partial-signed transaction with our `SignAndSendTransaction` method.

```csharp
var address = "SOLANA_ADDRESS";
var wallet = new Wallet(_mnemonicWords);
var signer = wallet.GetAccount(1);

var transaction = new Transaction();
... // transaction manipulation

var convertedTransaction = bloctoWalletProvider.ConvertToProgramWalletTransaction(address, transaction);

convertedTransaction.PartialSign(new List<Account>{ signer });
bloctoWalletProvider.SignAndSendTransaction(address, convertedTransaction, txhash => {
                                                                                 Debug.Log($"Tx hash: {txhash}");
                                                                           });
```

\
