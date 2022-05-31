# Send Transaction

{% hint style="warning" %}
Make sure you [initialize Blocto SDK](getting-started.md) first
{% endhint %}

Once your app is connected to Blocto wallet, it can send transactions on behalf of the user, with the user's permission.

In order to send a transaction, the app needs to:

* Create **data** which is the content of transacting with a smart contract
* Create **value** if needs to send the Ether to smart contract

| Parameter            | Type                     | Description                                                                                                                     |
| -------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| **fromAddress**      | address string           | the sender address                                                                                                              |
| **toAddress**        | address string           | the smart contract address                                                                                                      |
| **data**             | hex string (`0x` prefix) | the content of transacting with a smart contract, can use library, such as [Web3j](https://github.com/web3j/web3j), to generate |
| **value** (optional) | BigInteger               | the amount of Ether deposit in the smart contract (if the smart contract accepts ether)                                         |

```
// Create data using web3j
val donateFunction = Function(
    "donate", 
    listOf(Utf8String("Hi, Blocto")), 
    emptyList()
)
val data = FunctionEncoder.encode(donateFunction)

// Create value using web3j
val value = Convert.toWei("1", Convert.Unit.ETHER).toBigInteger()

// Send transaction based on specific chain
BloctoSDK.[ethereum/bnb/polygon/avalanche].sendTransaction(
    context = context,
    fromAddress = fromAddress,
    toAddress = toAddress,
    data = data,
    value = value,
    onSuccess = { txHash ->
        // transaction sent
    },
    onError = { error ->
        // handle error
    }
)
```
