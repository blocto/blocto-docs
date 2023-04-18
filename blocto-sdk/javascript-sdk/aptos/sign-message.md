# Sign Message

Once the web application is connected to Blocto wallet, it can sign the message on behalf of the user, with the user's permission.

In order to sign a message, the web application must:

* Create a sign message request payload.
* Have it be signed by the user's Blocto wallet.

#### **Sign a message**

```javascript
const response = await aptos.signMessage({
  message: "Hello world",
  nonce: "foo", // a unique identifier for the request
})
const { signature } = response // an array of signatures of the multi-sig account
```

#### **Verify signatures**

After getting the signature, you might want to verify it to see whether it's valid.\
\
For this purpose, we need to figure out the keys that we used to sign the message since Blocto wallet creates multi-signers account for you.\
\
By checking with the bitmap in the response you got, you can get the public keys that we need for verifying the signatures.

```javascript
// a 4-byte (32 bits) bit-vector of length N
const { bitmap } = response

const input = new Uint8Array(bitmap)
// Getting an array which marks the keys signing the message with 1, while marking 0 for the keys not being used.
const bits = Array.from(input).flatMap((n) =>
  Array.from({ length: 8 }).map((_, i) => (n >> i) & 1)
)
// Filter out indexes of the keys we need
const index = bits.map((_, i) => i).filter((i) => bits[i]) 

const { publicAccount: { publicKey } } = bloctoSDK.aptos
// The filter result is an array of the keys which we need for verifying the signatures
const publicKeys = publicKey.filter((_: string, i: number) => index.includes(i))
```

Here we use [tweetnacl](https://www.npmjs.com/package/tweetnacl) for verifying the message. For each signature, we need to verify with the corresponding public key we just got by reading bitmap. Every signature should pass the verification.

```javascript
import nacl from 'tweetnacl';

// ...
const { fullMessage, signature } = response

for (let i = 0; i < signature.length; i++) {
  const isVerfied = nacl.sign.detached.verify(
    Buffer.from(fullMessage),
    Buffer.from(signature[i], 'hex'),
    Buffer.from(publicKeys[i], 'hex')
  ); // `isVerfied` should be `true` for every signature
}
```

#### Demo

{% embed url="https://codesandbox.io/embed/aptos-sign-message-and-verify-321x74?fontsize=14&hidenavigation=1&theme=dark" %}
