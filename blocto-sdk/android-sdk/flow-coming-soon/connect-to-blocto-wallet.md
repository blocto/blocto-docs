---
description: Connecting to wallet is being called authentication or authn in FCL
---

# Connect to Blocto Wallet

{% hint style="warning" %}
Make sure you have set the configuration in [Wallet Discovery](wallet-discovery.md) first.
{% endhint %}

By connecting to a wallet, a user's Flow account address can be retrieved. There are two different ways to connect a wallet:

1. Simple login: Retrieve account address **without** account proof data
2. Account-proof login: Retrieve account address **with** account proof data

{% hint style="info" %}
Account proof data can be used to prove a user controls an on-chain account
{% endhint %}

### Simple Login

```kotlin
when (val result = Fcl.login()) {
    is Result.Success -> {
        val address = result.value
    }
    is Result.Failure -> {
        // Handle error
    }
}
```

### Account-proof Login

If a dApp asks the user for authentication with account proof data,  the user will be asked to approve signing a message. It will return user's flow account address with `AccountProofData` which can be used to prove the ownership of a Flow account.

```kotlin
val accountProof = AccountProofResolvedData(
    appIdentifier = "YOUR_FLOW_APP_IDENTIFIER", // human-readable string i.e. the name of your app 
    nonce = "75f8587e5bd5f9dcc9909d0dae1f0ac5814458b2ae129620502cb936fde7120a" // minimum 32-byte random nonce as a hex string
)

// suspending function 
when (val result = Fcl.authenticate(accountProof)) {
    is Result.Success -> {
        val address = result.value
    }
    is Result.Failure -> {
        // Handle error
    }
}

// Once you've authenticated with provided data,
// you can access account proof data
val userAccountProofData = Fcl.currentUser?.accountProofData
```

### Verify Account Proof

FCL has a utility function called `verifyAccountProof()`. It can be used to verify a signature against a Flow account given the account address.

```kotlin
// accountProofData is mandatory if you wish to verify account proof
val userAccountProofData = Fcl.currentUser?.accountProofData ?: throw Exception("")

when (val result = Fcl.verifyAccountProof(FLOW_APP_IDENTIFIER, accountProofData)) {
    is Result.Success -> {
        val isValid = reuslt.value
    }
    is Result.Failure -> {
        // Handle error
    }
}
```

{% hint style="warning" %}
The first parameter of `verifyAccountProof()`, `appIdentifier`, is identical with `AccountProofResolvedData` 's appIdentifier.&#x20;
{% endhint %}
