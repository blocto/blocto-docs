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
        
    case .failure(let error):
        
    }
}
```

{% hint style="info" %}
Make sure you initialize Blocto SDK first
{% endhint %}
