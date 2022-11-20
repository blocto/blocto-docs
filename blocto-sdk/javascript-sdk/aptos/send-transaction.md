# Send Transaction

Once the web application is connected to Blocto wallet, it can send transactions on behalf of the user, with the user's permission.

In order to send a transaction, the web application must:

* Create a transaction payload.
* Have it be signed by the user's Blocto wallet.
* Send it with Blocto custom RPC server.

**Sign and submit a Transaction**

{% hint style="warning" %}
Note that there're three types of transaction payload in aptos: **entry\_function**_**\_**_**payload, script\_payload and module\_bundle,** for the time being Blocto only supports **entry\_function\_payload** type.
{% endhint %}



```javascript
// transfer 1000 Octas to 0x123
const transaction = {
  arguments: ['0x123', '1000'],
  function: '0x1::coin::transfer',
  type: 'entry_function_payload',
  type_arguments: ['0x1::aptos_coin::AptosCoin'],
}
const options = {
  max_gas_amount: '20000' // default to '50000'
}
... 
// sign and submit the transaction & get the tx hash
const { hash } await bloctoSDK.aptos.signAndSubmitTransaction(transaction, options)
```

{% hint style="warning" %}
Gas unit price is not supported in options currently, we will use [estimate\_gas\_price](https://fullnode.mainnet.aptoslabs.com/v1/spec#/operations/estimate\_gas\_price) from Aptos full node api instead.
{% endhint %}
