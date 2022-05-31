# Connect to Blocto wallet

{% hint style="warning" %}
Make sure you [initialize Blocto SDK](../solana/getting-started.md) first
{% endhint %}

Before you start to interact with Blocto wallet, you need to have wallet connected.

Once the wallet connection requested, it would

* redirect to the **Blocto wallet app** if it is installed.
* open Blocto web SDK using ASWebAuthenticationSession if the **Blocto Wallet app** is not installed.

and ask user to connect the wallet.

```swift
BloctoSDK.shared.ethereum.requestAccount { [weak self] result in
    switch result {
    case .success(let address):
        // handle address here
    case .failure(let error):
        // handle error here
    }
}
```

```swift
BloctoSDK.shared.bsc.requestAccount { [weak self] result in
    switch result {
    case .success(let address):
        // handle address here
    case .failure(let error):
        // handle error here
    }
}
```

```swift
BloctoSDK.shared.polygon.requestAccount { [weak self] result in
    switch result {
    case .success(let address):
        // handle address here
    case .failure(let error):
        // handle error here
    }
}
```

```swift
BloctoSDK.shared.avalanche.requestAccount { [weak self] result in
    switch result {
    case .success(let address):
        // handle address here
    case .failure(let error):
        // handle error here
    }
}
```
