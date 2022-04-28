# Getting started

In this guide we show how to use it.

A sample application is available at: [https://github.com/portto/blocto-ios-sdk/tree/main/Example](https://github.com/portto/blocto-ios-sdk/tree/main/Example)

## Installation

An easy way to add Blocto iOS SDK dependency to an iOS project is through _CocoaPods_, like this:

```ruby
pod 'BloctoSDK/Solana'
```

The dependency can be installed simply by `pod install`:

```ruby
pod install
```

## Usage

#### Initialize Blocto SDK

```swift
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
parameter `testnet`: specify the cluster. `true` for **mainnet-beta** and `false` for **devnet**. Default is `false.`
{% endhint %}



#### UIApplicationDelegate delegate method implementation

```swift
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

```swift
func application(
    _ application: UIApplication,
    continue userActivity: NSUserActivity,
    restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void
) -> Bool {
    BloctoSDK.shared.continue(userActivity)
    return true
}
```
