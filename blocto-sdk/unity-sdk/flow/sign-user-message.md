# Sign User Message

{% hint style="warning" %}
Make sure you have set the configuration in [Getting Started](getting-started.md#configuration) first.
{% endhint %}

Use of cryptographic signatures is a key part of the blockchain. They are used to prove ownership of an address without exposing its private key. While primarily used for signing transactions, cryptographic signatures can also be used to sign arbitrary messages.

FCL allows you to send an arbitrary string to a wallet provider where the user may approve signing it with their private keys.

```csharp
using Flow.FCL;
using Flow.Net.Sdk.Core.Models;

var userSignature = default(FlowSignature);
var originalMessage = "SignMessage Test";
fcl.SignUserMessage(
    message: originalMessage,
    callback: result => {
                  if(result.IsSuccessed == false)
                  {
                      Debug.Log($"Get signmessage failed, Reason: {result.Message}");
                      return;
                  }
                
                  userSignature = result.Data;
                  Debug.Log($"Message: {originalMessage} \r\n");
                  foreach (var userSignature in result.Data)
                  {
                      Debug.Log($"Signature: {Encoding.UTF8.GetString(userSignature.Signature)} \r\nKeyId: {userSignature.KeyId}\");
                  }
              }); 
```

Unlike account proof, `fcl` will NOT hold that user signature for you. You have to store it yourself for verification.

{% hint style="warning" %}
We can retrieve user signatures only after user had logged in, otherwise error will be thrown.
{% endhint %}

The message could be signed by several private key of the same wallet address. Those signatures will be valid all together as long as their corresponding key weight sum up at least 1000. That's why return value above is an array.

#### Verify user signatures

Just like account proof, FCL-Unity includes a utility function in `AppUtility`, `verifyUserSignatures`, for verifying one or more user signatures against an account's public key on the Flow blockchain.

```csharp
using Flow.FCL;
using Flow.Net.Sdk.Core.Models;

var userSignature = ...

var appUtil = new AppUtility(gameObject);
var isVerify = appUtil.VerifyUserSignatures(
        message: originalMessage, 
        signature: userSignature,
        fclCryptoContract: BLOCTO_FCLCRYPTO_CONTRACT_ADDRESS
    );

Debug.Log($"Verify result: {isVerify}");
```

BLOCTO\_FCLCRYPTO\_CONTRACT\_ADDRESS can be found [here](../../javascript-sdk/flow/account-proof.md)
