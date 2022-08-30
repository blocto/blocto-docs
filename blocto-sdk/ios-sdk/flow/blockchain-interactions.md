# Blockchain Interactions

{% hint style="warning" %}
Make sure you set the configuration in [Getting started](getting-started.md) first.
{% endhint %}

We are assuming you have read the [Scripts Documentation](https://docs.onflow.org/fcl/reference/scripts/) before this, as transactions are sort of scripts with more required things.

There are two different operations to interact with Flow blockchain.

* _Query the chain_: Send arbitrary Cadence scripts to the chain and receive back decoded values. Can be performed without user login.

```swift
import FCL_SDK

let script = """
import ValueDapp from \(valueDappContract)

pub fun main(): UFix64 {
    return ValueDapp.value
}
"""

Task {
    let argument = try await fcl.query(script: script)
    label.text = argument.value.description
    
    // decode as Swift `Decodable` type or primitive type
    let value: Decimal = argument.value.toSwiftValue()
}
```

* _Mutate the chain_: Use transaction to send Cadence code with specify authorizer to perform permanently state changes on chain. Must be perform after user logged in.

```swift
import FCL_SDK

// 1. login or authanticate first.

// 2.
Task { @MainActor in
    guard let userWalletAddress = fcl.currentUser?.address else {
        // handle error
        return
    }

    let scriptString = """
    import ValueDapp from 0x5a8143da8058740c

    transaction(value: UFix64) {
        prepare(authorizer: AuthAccount) {
            ValueDapp.setValue(value)
        }
    }
    """

    let argument = Cadence.Argument(.ufix64(10))

    let txHsh = try await fcl.mutate(
        cadence: scriptString,
        arguments: [argument],
        limit: 100,
        authorizers: [userWalletAddress]
    )
}
```

While `query` is used for sending scripts to the chain, `mutate` is used for building and sending transactions. Just like [scripts](https://docs.onflow.org/fcl/reference/scripts/).

### Transaction status

In order to check transaction status, we can use `getTransactionStatus` in `fcl` to polling until transaction have been sealed into block.

```swift
import FCL_SDK

Task {
    do {
        let result = try await fcl.getTransactionStatus(transactionId: txHash)
    } catch {
        // handle error here.
    }
}
```

After the status changes to sealed, we can query the Flow blockchain to see if the transaction works as expected.
