# Sign Message

{% hint style="warning" %}
Make sure you [initialize Blocto SDK](getting-started.md) first
{% endhint %}

Once your app is connected to Blocto wallet, it can sign message on behalf of the user, with the user's permission.

To sign a message, the app must specify the sign type. All available types are listed below.

| Sign Type                                                                | Type                        | Message                  |
| ------------------------------------------------------------------------ | --------------------------- | ------------------------ |
| ETH Sign                                                                 | `EvmSignType.ETH_SIGN`      | hex string (`0x` prefix) |
| Personal Sign                                                            | `EvmSignType.PERONSAL_SIGN` | plain string             |
| Sign Typed Data v3                                                       | `EvmSignType.TYPED_DATA_V3` | json string              |
| Sign Typed Data v4                                                       | `EvmSignType.TYPED_DATA_V4` | json string              |
| <p>Sign Typed Data</p><p>(currently identical to Sign Typed Data v4)</p> | `EvmSignType.TYPED_DATA`    | json string              |

```kotlin
// Sign message based on specific chain

BloctoSDK.[ethereum/bnb/polygon/avalanche].signMessage(
    context = context,
    fromAddress = address,
    signType = [EvmSignType.ETH_SIGN/EvmSignType.PERSONAL_SIGN/...],
    message = message,
    onSuccess = { signature ->
        // get signature
    },
    onError = { error ->
        // handle error
    }
)
```

For dApps relying on `signMessage` for off-chain authentication, Blocto follows [EIP-1654](https://github.com/ethereum/EIPs/issues/1654). To verify the signature, you need to call a method on the wallet contract to check if the signature came from a rightful owner of the wallet contract.

```kotlin
// Example code by using web3j

// hash: hash of data (message) to be signed
// signature: signature byte array associated with hash

fun verifySignature(hash: ByteArray, signature: ByteArray): Boolean {
    val web3j = Web3j.build(HttpService(rpcUrl))
    val function = Function(
        "isValidSignature",
        listOf(Bytes32(hash), DynamicBytes(signature)),
        listOf<TypeReference<Bytes4>>()
    )
    val encodedFunction = FunctionEncoder.encode(function)
    return web3j.ethCall(
        Transaction.createEthCallTransaction(
            address,
            address,
            encodedFunction
        ),
        DefaultBlockParameterName.LATEST
    ).send().value.orEmpty() == ERC1271_MAGIC_VALUE
}
```
