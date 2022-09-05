# Sign User Message

{% hint style="warning" %}
Make sure you set the configuration in [Getting started](getting-started.md) first.
{% endhint %}

Use of cryptographic signatures is a key part of the blockchain. They are used to prove ownership of an address without exposing its private key. While primarily used for signing transactions, cryptographic signatures can also be used to sign arbitrary messages.

FCL has a feature that let you send arbitrary string to a wallet provider where the user may approve signing it with their private keys.

```swift
import FCL_SDK

// 1. login or authanticate first.

// 2. 
Task {
    do {
        let signatures: [FCLCompositeSignature] = try await fcl.signUserMessage(message: "message you want user to sign.")
    } catch {
        // handle error
    }
}
```

Unlike account proof, `fcl` will NOT hold that user signature for you. You have to store it yourself for verification.

{% hint style="warning" %}
We can retrieve user signatures only after user had logged in, otherwise error will be thrown.
{% endhint %}

The message could be signed by several private key of the same wallet address. Those signatures will be valid all together as long as their corresponding key weight sum up at least 1000. That's why return value above is an array.

### Verify user signatures

Just like account proof, FCL-Swift includes a utility function in `AppUtilities`, `verifyUserSignatures`, for verifying one or more user signatures against an account's public key on the Flow blockchain.

```swift
import FCL_SDK
import BloctoSDK

var userSignatures: [FCLCompositeSignature] = ...

Task {
    do {
        let valid = try await AppUtilities.verifyUserSignatures(
            message: Data(message.utf8).bloctoSDK.hexString,
            signatures: userSignatures,
            fclCryptoContract: Address(hexString: BLOCTO_FCLCRYPTO_CONTRACT_ADDRESS)
        )
    } catch {
        // handle error here.
    }
}
```

BLOCTO\_FCLCRYPTO\_CONTRACT\_ADDRESS can be found [here](../../javascript-sdk/flow/account-proof.md)

{% hint style="info" %}
Blocto SDK has helper function to make data into hex string, just import `BloctoSDK` to use it.
{% endhint %}
