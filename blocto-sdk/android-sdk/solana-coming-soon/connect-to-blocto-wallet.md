# Connect to Blocto wallet

{% hint style="warning" %}
Make sure you [initialize Blocto SDK](getting-started.md) first
{% endhint %}

Before you start to interact with Blocto wallet, you need to have wallet connected.

Once the wallet connection requested, it would

* redirect to the **Blocto wallet app** if it is installed
* open webview if the app is not installed

and ask user to connect the wallet.

```kotlin
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
