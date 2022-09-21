---
description: Connecting to wallet is being called authentication or authn in FCL
---

# Connect to Blocto Wallet

{% hint style="warning" %}
Make sure you have set the configuration in [Getting Started](getting-started.md) first.
{% endhint %}

There are two different ways to connect wallet:

* Normal log in and retrieve user's flow account address.

```swift
import FCL_SDK

Task {
    let address = try await fcl.login()
}
```

* Log in with account proof data to retrieve user's flow account address along with account proof.

&#x20;If dApp chooses to ask user for user's authentication in this way, and the user approves the signing of message data, it will return user's flow account address with `accountProofData` that can be used to prove a user controls an on-chain account.

```swift
import FCL_SDK

let accountProofData = FCLAccountProofData(
    appId: "Here you can specify your app name.",
    nonce: "75f8587e5bd5f9dcc9909d0dae1f0ac5814458b2ae129620502cb936fde7120a" // minimum 32-byte random nonce as a hex string.
)
Task {
    do {
        let address = try await fcl.authanticate(accountProofData: accountProofData)
    } catch {
        // handle error here
    }
}
```

`fcl.authanticate` is also called behide `fcl.login()` with `accountProofData` set to nil.

Both methods above store address under `fcl.currentUser` , to get user address just simply call as below.

```swift
if let userWalletAddress = fcl.currentUser?.address {
    // do some stuff here.
}
```

To get user address or account proof after user authenticated, just simply call as below.

```swift
if let accountProof = fcl.currentUser?.accountProof {
    // verify account proof here.
}
```

### Verify account proof

Cadence has a built-in function called verify that will verify a signature against a Flow account given the account address.

FCL-Swift includes a utility function in `AppUtilities`, `verifyAccountProof`, for verifying one or more account proof signatures against an account's public key on the Flow blockchain.

```swift
import FCL_SDK

guard let accountProof = fcl.currentUser?.accountProof else {
    // handle error here
    return
}

Task {
    do {
        let valid = try await AppUtilities.verifyAccountProof(
            appIdentifier: "Here you can specify your app name.",
            accountProofData: accountProof,
            fclCryptoContract: Address(hexString: BLOCTO_FCLCRYPTO_CONTRACT_ADDRESS)
        )
    } catch {
        // handle error here
    }
}
```

BLOCTO\_FCLCRYPTO\_CONTRACT\_ADDRESS can be found [here](../../javascript-sdk/flow/account-proof.md)

{% hint style="warning" %}
input parameter `appIdentifier` should be exactly same with `FCLAccountProofData` 's appId above. They should all be a human readable string.
{% endhint %}
