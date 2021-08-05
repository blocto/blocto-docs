# Connect to Blocto wallet

Before you started to interact with blocto wallet, you need to fire a connect request.

Once the connection request is fired, there would be a prompt modal to guide user to register/login to Blocto wallet.

{% tabs %}
{% tab title="connect\(\)" %}
```javascript
// Alternative: EIP-1102 way
// CAVEATS! it's deprecated and may be removed from future version
const accounts = await bloctoSDK.solana.connect();
```
{% endtab %}

{% tab title="request\(\)" %}
```javascript

// EIP-1193 way (recommended)
const accounts = bloctoSDK.solana.request({ method: 'connect' });
```
{% endtab %}
{% endtabs %}



