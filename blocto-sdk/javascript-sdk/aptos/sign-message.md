# Sign message

Once the web application is connected to Blocto wallet, it can sign the message on behalf of the user, with the user's permission.

In order to sign a message, the web application must:

* Create a sign message request payload.
* Have it be signed by the user's Blocto wallet.

**Sign a message**



```javascript
const response = await aptos.signMessage({
  message: "Hello world",
  nonce: "foo", // a unique identifier for the request
})
const { signature } = response // an array of signatures of the multi-sig account
```
