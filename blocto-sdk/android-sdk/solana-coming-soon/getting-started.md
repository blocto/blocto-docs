# Getting started

### Installation

Add the dependency below to your module's `build.gradle` file

```
dependencies {
    implementation "com.portto.sdk:solana:0.1.0"
}
```

### Usage

Initialize Blocto SDK

```
BloctoSDK.init(
    appId = "YOUR_APP_ID", // required
    debug = true           // optional (default is false)
)
```

{% hint style="info" %}
`debug`: Specify the cluster. `true` for **mainnet-beta** and `false` for **devnet**. Default is `false`.
{% endhint %}
