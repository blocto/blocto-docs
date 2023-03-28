# Send Transaction

### SendTransaction

Once your app is connected to Blocto wallet, it can send transactions on behalf of the user, with the user's permission. To send a transaction, the app must:

* create an unsigned transaction.
* send the unsigned transaction to Blocto.

In Aptos, Transaction Payload can be divided into the following two types:

* Entry Function Payload: a transaction payload that can call a Module on the blockchain.
* Script Payload: a transaction payload that includes a script.

Currently, the SendTransaction method in Unity SDK supports both structures. The following is an example code to create a Transaction Payload.

Entry Function Payload

```csharp
var transaction = new EntryFunctionTransactionPayload
                      {
                          Address = "your address", 
                          Function = "0x1::coin::transfer",
                          Arguments = new object[] { "0x49e39c396ee0f1ace3990968cd9991b6bec0aa6f329afc9d1fa79757839a703f", "100000000" },
                          TypeArguments = new string[] { "0x1::aptos_coin::AptosCoin" },
                      };
```

Script Payload

```csharp
var transaction = new ScriptTransactionPayload
                      {
                          Address = "your address",
                          Arguments = new object[] { "0x49e39c396ee0f1ace3990968cd9991b6bec0aa6f329afc9d1fa79757839a703f", "1" },
                          TypeArguments = new string[]{ },
                          Code = new Code
                                 {
                                     ByteCode = "0xa11ceb0b0500000005010002030205050706070d170824200000000100010003060c0503000d6170746f735f6163636f756e74087472616e736665720000000000000000000000000000000000000000000000000000000000000001000001050b000b010b02110002", Abi = new AbiPayload
                                           {
                                               Name = "main",
                                               Visibility = "public",
                                               IsEntry = "true",
                                               GenericTypeParams = new string[]{ },
                                               Params = new[]{ "&signer", "address", "u64" },
                                               Return = new string[] { }
                                           }
                                 }
                      };
```

You can then use the SendTransaction provided by the Unity SDK to send the Transaction Payload to Blocto for signing and sending it to the blockchain.

<pre class="language-csharp"><code class="lang-csharp"><strong>_bloctoWalletProvider.SendTransaction(transaction, tx => {
</strong>                                                            Debug.Log($"Tx is {tx}");
                                                        });
</code></pre>
