# Sign User Message

{% hint style="warning" %}
Make sure you have set the configuration in [Getting Started](getting-started.md) first.
{% endhint %}

In blockchain, signatures are used to prove the ownership of an address without exposing its private key. While primarily used for signing transactions, cryptographic signatures can also be used to sign arbitrary messages.

FCL allows you to send an arbitrary string to a wallet provider where the user may approve signing it with their private keys.

```kotlin
when (val result = Fcl.signUserMessage("Your message to sign")) {
    is Result.Success -> {
        val userSignatures = result.value
    }
    is Result.Failure -> {
        // Handle error
    }
}
```

Unlike account proof, FCL does NOT hold that user signatures for you. You have to store it yourself, if you wish to retrieve user signatures later for verification.&#x20;

### Verify User Signatures

Similar to account proof, FCL provides `verifyUserSignatures()` to verify one or more user signatures against an account's public key on the Flow blockchain.

```kotlin
when (val result = Fcl.verifyUserSignatures("Your message to sign", userSignatures)) {
    is Result.Success -> {
        val isValid = reuslt.value
    }
    is Result.Failure -> {
        // Handle error 
    }
}
```

{% hint style="info" %}
`signUserMessage()` and `verifyUserSignatures()` share the same message.
{% endhint %}
