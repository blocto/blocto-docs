# Getting Started

### Installation

Add the dependency below to your module's `build.gradle` file

```groovy
dependencies {
    implementation "com.portto.fcl:$fclVersion"
}
```

### Initialization

To initialize FCL, use the following function:

```kotlin
Fcl.init(
    env = Network.TESTNET,
    appDetail = AppDetail(),
    supportedWallets = walletProviderList
)
```

{% hint style="info" %}
* env: The network on which the app was built. i.e. `Network.TESTNET` or `Network.MAINNET`
* appDetail: The information of the app.
* supportedWallets: A list of **wallet providers** supported by the app. See **Wallet Discovery** for more info.
{% endhint %}
