# Connect to Blocto wallet

Before you start to interact with Blocto wallet, you need to have wallet connected.

Once the wallet connection requested, it would

* redirect to the **Blocto wallet app** if it is installed
* open Web SDK page using ASWebAuthenticationSession

and ask user to connect the wallet.

```
BloctoSDK.shared.solana.requestAccount { [weak self] result in
    switch result {
    case .success(let address):
        // handle address here
    case .failure(let error):
        // handle error here
    }
}
```

{% hint style="warning" %}
Make sure you [initialize Blocto SDK](https://docs.blocto.app/blocto-sdk/ios-sdk/solana-coming-soon/getting-started#initialize-blocto-sdk) first
{% endhint %}
