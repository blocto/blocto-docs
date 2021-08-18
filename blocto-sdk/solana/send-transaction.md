# Send Transaction

Once the web application is connected to Blocto wallet, it can send transactions on behalf of the user, with the user's permission.

In order to send a transaction, the web application must:

* Create an unsigned transaction or transactions.
* Have it be signed by the user's Blocto wallet.
* Send it with Blocto custom JSON-RPC.

For more information about the transactions on Solana, it is recommended to check out the `solana-web3.js` [docs](https://solana-labs.github.io/solana-web3.js/classes/Transaction.html) as well as the official [Solana docs](https://docs.solana.com/developing/programming-model/transactions).

**Plain Transaction**

For plain transactions \(no dApp-side signing involved\), you can just create transaction with `solana-web3.js` and sign-and-send the transaction with `signAndSendTransaction` method.

{% tabs %}
{% tab title="signAndSendTransaction\(\)" %}
```javascript
const transaction = new solanaWeb3.Transaction();
...
const txHash = await bloctoSDK.solana.signAndSendTransaction(transaction);
```
{% endtab %}

{% tab title="request\(\)" %}
```javascript
const transaction = new solanaWeb3.Transaction();
...
bloctoSDK.solana.request({
  method: 'signAndSendTransaction',
  params: {
    message: transaction.serializeMessage().toString('hex')
  }
});
```
{% endtab %}
{% endtabs %}

**Partial Sign Transaction**

For transactions involving dApp-side signing, first you need to convert the transaction to our wallet-compatible format by calling `convertToProgramWalletTransaction`, and sign the instructions with your keys, then sign-and-send the partial-signed transaction with our `signAndSendTransaction` method.

{% tabs %}
{% tab title="convertToProgramWalletTransaction\(\)" %}
```javascript
const transaction = new solanaWeb3.Transaction();
...
const converted = await bloctoSDK.solana.convertToProgramWalletTransaction(transaction);
const signers = [new solanaWeb3.PublicKey('yourpublickey')];
converted.partialSign(...signers);

const txHash = await bloctoSDK.solana.signAndSendTransaction(converted);
```
{% endtab %}

{% tab title="request\(\)" %}
```javascript
const transaction = new solanaWeb3.Transaction();
...
const convertedMessage = await bloctoSDK.solana.request({
  method: 'convertToProgramWalletTransaction',
  params: {
    message: transaction.serializeMessage().toString('hex')
  }
});
// convert message to solana Transaction type object
const converted = bloctoSDK.solana.toTransaction(convertedMessage);
const signers = [new solanaWeb3.PublicKey('yourpublickey')];
converted.partialSign(...signers);

const txHash = await bloctoSDK.solana.request({
  method: 'signAndSendTransaction',
  params: {
    signatures: await solanaSDK.solana.collectSignatures(converted),
    message: converted.serializeMessage().toString('hex')
  }
});
```
{% endtab %}
{% endtabs %}

