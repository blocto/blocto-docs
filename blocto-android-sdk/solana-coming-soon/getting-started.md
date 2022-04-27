# Getting started

{% hint style="warning" %}
Note that Blocto Android SDK for Solana is still in **Beta**.\
APIs are subject to breaking changes.
{% endhint %}

### Register an App ID

1. Go to Developer Dashboard ([production](https://developers.blocto.app), [staging](https://developers-staging.blocto.app))
2. Login with Blocto account or register a new one
3. Register an app to get app id

{% hint style="info" %}
Based on development environment, you need to download the corresponding Blocto app for testing:

* [production](https://play.google.com/store/apps/details?id=com.portto.blocto)
* [staging](https://play.google.com/store/apps/details?id=com.portto.blocto.staging)
{% endhint %}

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
