# Connect to Blocto wallet

Before you start to interact with Blocto wallet, you need to have wallet connected.

Once the wallet connection requested, it would

* redirect to the **Blocto wallet app** if it is installed
* open webview if the app is not installed

and ask user to connect the wallet.

```
BloctoSDK.solana.requestAccount(
    context = this,
    onSuccess = { address ->
        // wallet connected
    },
    onError = { error ->
        // handle error
    }
)
```
