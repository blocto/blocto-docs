---
description: Connecting to wallet is being called authentication or authn in FCL
---

# Connect to Blocto Wallet

{% hint style="warning" %}
Make sure you have set the configuration in [Getting Started](getting-started.md#configuration) first.
{% endhint %}

There are two different ways to connect wallet:

* Normal log in and retrieve user's flow account address.

```csharp
using Flow.FCL;

fcl.Login();
```

or

```csharp
using Flow.FCL;

fcl.Login((currentUser,  accountProofData) => {
                       Debug.Log(currentUser.Addr.Address.AddHexPrefix());
                   });
```

When login completed, will call lambda to update UI if you have passing lambda parameter.

* Log in with account proof data to retrieve user's flow account address along with account proof.

If dApp chooses to ask user for user's authentication in this way, and the user approves the signing of message data, it will return user's flow account address with `accountProofData` that can be used to prove a user controls an on-chain account.

```csharp
using Flow.FCL;

var accountProofData = new AccountProofData
                       {
                           AppId = "{your app name}",
                           Nonce = KeyGenerator.GetUniqueKey(32).StringToHex()
                       };
fcl.Authenticate(
    accountProofData: accountProofData,
    callback: ((currentUser,  accountProofData) => {
                   Debug.Log(currentUser.Addr.Address.AddHexPrefix());
               }));
```

`fcl.authanticate` is also called behide `fcl.login()` with `accountProofData` set to null.

Both method above store address under `fcl.currentUser` , you can easy to get user address or account proof data just simply call as below.

```csharp
var userWalletAddress = fcl.CurrentUser()?.Addr.Address;
```

To get user address or account proof after user authenticated, just simply call as below.

```csharp
var accountProof = fcl.CurrentUser()?.AccountProofData;
```

#### Verify account proof

Cadence has a built-in function called verify that will verify a signature against a Flow account given the account address.

FCL-Unity includes a utility function in `AppUtility`, verifyAccountProof, for verifying one or more account proof signatures against an account's public key on the Flow blockchain.

```csharp
using Flow.FCL;

fcl.Authenticate(
   accountProofData: accountProofData, 
   callback: ((currentUser,  accountProofData) => {
                  var appUtil = new AppUtility(gameObject, new ResolveUtility());
                  var isVerify = appUtil.VerifyAccountProofSignature(
                      appIdentifier: accountProofData!.AppId,
                      accountProofData: accountProofData,
                      fclCryptoContract: BLOCTO_FCLCRYPTO_CONTRACT_ADDRESS);
              }));
```

BLOCTO\_FCLCRYPTO\_CONTRACT\_ADDRESS can be found [here](../../javascript-sdk/flow/account-proof.md)

{% hint style="warning" %}
input parameter `appIdentifier` should be exactly same with `AccountProofData` 's appId above. They should all be a human readable string.
{% endhint %}
