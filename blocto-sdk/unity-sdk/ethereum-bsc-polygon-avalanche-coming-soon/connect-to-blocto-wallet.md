# Connect to Blocto wallet

## Connect to Blocto Wallet

{% hint style="info" %}
Make sure you [initialize Blocto SDK](getting-started.md) first
{% endhint %}

Before you start to interact with Blocto wallet, you need to have your wallet connected.

Once the wallet connection is requested, it would

* redirect to the **Blocto wallet app** if it is installed.
* open Blocto web SDK using ASWebAuthenticationSession if the **Blocto Wallet app** is not installed.

and ask user to connect the wallet.

```csharp
bloctoWalletProvider.RequestAccount(address => {
                                           Debug.Log($"Address: {address}");
                                    });
```
