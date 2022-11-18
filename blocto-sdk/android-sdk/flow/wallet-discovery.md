---
description: Create a UI of wallets for user to log in
---

# Wallet Discovery

To allow the user to choose a wallet from the supported wallets you've added during FCL initialization, FCL provides Discovery for developers to focus on building their dApps.

There are two ways an app can use Discovery:

1. Discovery UI: A wallet list applying Material Design
2. Config: A list of wallets for custom UI

### Discovery UI

Discovery UI is the simplest way to integrate Wallet Discovery.  By calling `showConnectWalletDialog()`, FCL presents a list of wallet providers for a user to log in.

```kotlin
// MainActivity.kt
showConnectWalletDialog(this) {
    // authentication on user click
}
```

{% hint style="info" %}
In order to use the new Material Design 3 themes, you need to add Material Design version 1.5.0 or later.\
[Learn more about Material Design 3](https://m3.material.io/)
{% endhint %}

### Config

FCL allows the developer to control over the authentication UI. You can access the wallet providers   directly from `Fcl.config.supportedWallets`.

```kotlin
// Create your own UI by supplying wallet provider list as data to an adapter
val providerList = Fcl.config.supportedWallets

// Once you get the user's selected provider, you need to add it to config
Fcl.config.put(Config.Option.SelectedWalletProvider(provider))
```
