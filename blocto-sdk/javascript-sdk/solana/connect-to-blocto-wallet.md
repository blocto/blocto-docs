# Connect to Blocto wallet

Before you started to interact with blocto wallet, you need to fire a connect request.

Once the connection request is fired, there would be a prompt modal to guide user to register/login to Blocto wallet.

```javascript
import BloctoSDK from "@blocto/sdk";

const bloctoSDK = new BloctoSDK({
  solana: {
    net: "devnet"
  }
});
const connectWallet = async () {
    try {
        const accounts = await bloctoSDK?.solana?.request({ method: "connect" }); // wallet address
    } catch (error) {
        console.error(error)
    }
}

const disconnectWallet = async () {
    try {
        await bloctoSDK?.solana?.request({ method: "disconnect" });
    } catch (error) {
        console.error(error)
    }
}
```

### Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk/tree/main/examples/with-solana-blocto-connect" %}
