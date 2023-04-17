# Sign User Message

Once the web application is connected to Blocto wallet, it can sign the message on behalf of the user, with the user's permission.

In order to sign a message, the web application must:

* Create a sign message request payload.
* Have it be signed by the user's Blocto wallet.

**Sign a message**

```csharp
var signMessageRequest = new SignMessagePreRequest
                                 {
                                     Address = {your wallet address},
                                     Message = "Hi, blocto",
                                     Nonce = "1A2B3C", //random string to sign the message with
                                     IsIncludeAddress = false,
                                     IsIncludeApplication = false,
                                     IsIncludeChainId = false
                                 };
_bloctoWalletProvider.SignMessage(signMessageRequest, signMessageResponse => {
                                                          _signMessageResponse = signMessageResponse;
                                                          Debug($"SignMessage response: {JsonConvert.SerializeObject(_signMessageResponse)}");
                                                      });
```

**Verify signatures**

After getting the signature, you might want to verify it to see whether it's valid. For this purpose, we need to figure out the keys that we used to sign the message since Blocto wallet creates multi-signers account for you. By checking with the bitmap in the response you got, you can get the public keys that we need for verifying the signatures.

```json
// SignMessage Response
{
    "signatureId": string,
    "signature": string[], // signed full message
    "bitmap": int[], // a 4-byte (32 bits) bit-vector of length N
    "fullMessage": string, // The message that was generated to sign
    "message": string, // The message passed in by dApp
    "nonce": string, // Random string to sign the message with
    "prefix": string, // Should always be APTOS
    "address": string, // The address wrapped in the fullMessage
    "application": string, // The application wrapped in the fullMessage
    "chainId": int, // The chainId wrapped in the fullMessage
}
```

Here we use [Chaos.NaCl](https://github.com/CodesInChaos/Chaos.NaCl) for verifying the message. For each signature, we need to verify with the corresponding public key we just got by reading bitmap. Every signature should pass the verification.

```csharp
var publicKeys = _bloctoWalletProvider.PublicKeys(_walletAddress);
var bitmap = new BitArray(_signMessageResponse.Bitmap);
var keyIndexs = bitmap.Cast<bool>().Select((item, index) => item ? index : -1).Where(p => p >= 0).ToList();
var message = Encoding.UTF8.GetBytes(_signMessageResponse.FullMessage);
var isVerify = true;
for (var i = 0; i < _signMessageResponse.Signatures.Count; i++)
{
    var key = publicKeys[keyIndexs[i]].HexToBytes();
    var signatureBytes = _signMessageResponse.Signatures[i].HexToBytes();
    isVerify = Ed25519.Verify(signatureBytes, message, key);
}
  
Debug.Log($"Signature verify is {isVerify}");
```
