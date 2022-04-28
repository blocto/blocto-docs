# Getting started

In this guide we show how to use it.

A sample application is available at: [https://github.com/portto/blocto-ios-sdk/tree/main/Example](https://github.com/portto/blocto-ios-sdk/tree/main/Example)

## Adding Library Dependency

An easy way to add Blocto iOS SDK dependency to an iOS project is through _CocoaPods_, like this (the exact version may change in the future):

```
  pod 'BloctoSDK/Solana'
```

The dependency can be installed simply by `pod install`:

```shell
pod install
```

## Usage

#### Initialize Blocto SDK

```
if #available(iOS 13.0, *) {
    BloctoSDK.shared.initialize(
        with: "YOUR_APP_ID", // required
        window: yourWindow, // required PresentationContextProvider of web SDK authentication.
        logging: true, // optional (default is true)
        testnet: true // optional (default is false)
    )
} else {
    BloctoSDK.shared.initialize(
        with: "YOUR_APP_ID", // required
        logging: true, // optional (default is true)
        testnet: true // optional (default is false)
    )
}
```

{% hint style="info" %}
testnet: Specify the cluster. `true` for **mainnet-beta** and `false` for **devnet**. Default is `false`.
{% endhint %}



#### UIApplicationDelegate delegate method implementation

```
func application(
    _ app: UIApplication,
    open url: URL,
    options: [UIApplication.OpenURLOptionsKey: Any] = [:]
) -> Bool {
    BloctoSDK.shared.application(
        app,
        open: url,
        options: options)
    return true
}
```

```
func application(
    _ application: UIApplication,
    continue userActivity: NSUserActivity,
    restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void
) -> Bool {
    BloctoSDK.shared.continue(userActivity)
    return true
}
```
