# Send Transaction

{% hint style="info" %}
Make sure you [initialize Blocto SDK](getting-started.md) first
{% endhint %}

Once your app is connected to Blocto wallet, it can send transactions on behalf of the user, with the user's permission.

In order to send a transaction, the SDK provides two methods of signing for SendTransaction:

1. The[^1] app needs to:

* Create **data** which is the content of transacting with a smart contract
* Create **value** if needs to send the Ether to smart contract

| Parameter       | Type                     | Description                                                                                                                                 |
| --------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **fromAddress** | address string           | the sender address                                                                                                                          |
| **toAddress**   | address string           | the smart contract address                                                                                                                  |
| **value**       | BigInteger               | the amount of Ether deposit in the smart contract (if the smart contract accepts ether)                                                     |
| **data**        | hex string (`0x` prefix) | the content of transacting with a smart contract, can use library, such as [NEthereum](https://github.com/Nethereum/Nethereum), to generate |

<pre class="language-csharp"><code class="lang-csharp"><strong>var web3 = new Web3("{your node url}");
</strong>var contract = web3.Eth.GetContract("{your smart contract abi json}", "{your smart contract address}");
var function = contract.GetFunction("{Function name of the call}");
var data = function.GetData(new object[]{ "{function parameters}" });
bloctoWalletProvider.SendTransaction(
    fromAddress: address, 
    toAddress: "{your smart contract address}"
    value: 0, 
    data: data, 
    callback: txId => {
        Debug.Log("TxId: {txId}");
    });
</code></pre>

2. Using the `EvmTransaction` class. The SDK defines a Class that includes four properties: **From**, **To**, **Value**, and **Data**. Users can create an instance of this Class, set the property values accordingly, and then pass it to SendTransaction.

```csharp
var web3 = new Web3("{your node url}");
var contract = web3.Eth.GetContract("{your smart contract abi json}", "{your smart contract address}");
var function = contract.GetFunction("{Function name of the call}");
var data = function.GetData(new object[]{ "{function parameters}" });
var transaction = new EvmTransaction{
    From = address,
    To = "{your smart contract address}",
    Value = 0,
    Data = data
};

bloctoWalletProvider.SendTransaction(
    evmTransaction: transaction,
    callback: txId => {
        Debug.Log("TxId: {txId}");
    });
```

[^1]: 
