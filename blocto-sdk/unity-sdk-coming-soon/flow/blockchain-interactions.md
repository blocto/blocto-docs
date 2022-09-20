---
description: 'description: Query and mutate Flow blockchain'
---

# Blockchain Interactions

{% hint style="warning" %}
Make sure you have set the configuration in [Getting Started](getting-started.md#configuration) first.
{% endhint %}

We are assuming you have read the [Scripts Documentation](https://docs.onflow.org/fcl/reference/scripts/) before this, as transactions are sort of scripts with more required things.

There are two different operations to interact with Flow blockchain:

1. Query: Send arbitrary Cadence scripts to the chain and receive `ExecuteResult<ICadence>` values. Can be performed without user login
2. Mutate: Use transaction to send Cadence code with specify authorizer to perform permanently state changes on chain. Must be performed after user logged in

about `ICadance`, you can get more detail description from [Flow.Net](https://github.com/tyronbrand/flow.net#execute-scripts)

#### Query

```csharp
using Flow.FCL;
using Flow.Net.Sdk.Core.Models;

var script = @" 
            pub struct User {
                pub var balance: UFix64
                pub var address: Address
                pub var name: String
                
                init(name: String, address: Address, balance: UFix64) {
                    self.name = name
                    self.address = address
                    self.balance = balance
                }
            }

            pub fun main(name: String): User {
                return User(
                    name: name,
                    address: 0x1,
                    balance: 10.0
                )
            }";

var flowScript = new FlowScript
                    {
                        Script = script,
                        Arguments = new List<ICadence>
                                    {
                                        new CadenceString("blocto")
                                    }
                    };

var result = await fcl.QueryAsync(flowScript);
if(result.IsSuccessed)
{
    //// Composite object parser
    var name = result.Data.As<CadenceComposite>().CompositeFieldAs<CadenceString>("name").Value;
    var balance = result.Data.As<CadenceComposite>().CompositeFieldAs<CadenceNumber>("balance").Value;
    var address = result.Data.As<CadenceComposite>().CompositeFieldAs<CadenceAddress>("address").Value;
    et"Name: {name}, Balance: {balance}, Address: {address}";
}
else
{
    _resultTxt.text = result.Message;
}
```

#### Mutate

```csharp
using Flow.FCL;
using Flow.Net.Sdk.Core.Models;

var script = @"
            import ValueDapp from 0x5a8143da8058740c

            transaction(value: UFix64) {
                prepare(authorizer: AuthAccount) {
                    ValueDapp.setValue(value)
                }
            }";

var tx = new FlowTransaction
            {
                Script = script,
                GasLimit = 1000,
                Arguments = new List<ICadence>
                            {
                                new CadenceNumber(CadenceNumberType.UFix64, "123.456"),
                            },
            };

fcl.Mutate(tx, txId => {
                    Debug.Log($"https://testnet.flowscan.org/transaction/{txId}");
                });
```

While `query` is used for sending scripts to the chain, `mutate` is used for building and sending transactions.

#### Transaction status

In order to check transaction status, we can use `getTransactionStatus` in `fcl` to keep polling until transaction have been sealed into block.

```csharp
var result = _fcl.GetTransactionStatus(_txId);
if(result.IsSuccessed)
{
    switch (result.Data.Execution)
    {
        case TransactionExecution.Failure:
            Debug.Log($"Execution: Failure \r\nErrorMessage: {result.Message}");
            break;
        case TransactionExecution.Success:
            Debug.Log($"BlockId: {result.Data.BlockId} \r\nExecution: {result.Data.Execution} \r\nStatus: {result.Data.Status}");
            break;
        case TransactionExecution.Pending:
            Debug.Log($"{result.Message}");
            break;
    }
}
```

After the status changes to sealed, we can query the Flow blockchain to see if the transaction works as expected.
