# Getting Started

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

{% hint style="warning" %}
Blocto wallet supports Solana staring from version 3.7.0.
{% endhint %}

## Usage

#### Initialize Blocto SDK

```swift
BloctoSDK.shared.initialize(
    with: "YOUR_APP_ID", // required
    window: yourWindow, // required PresentationContextProvider of web SDK authentication.
    logging: true, // optional (default is true)
    environment: .dev // optional (default is prod)
)
```

{% hint style="info" %}
parameter `environment`: specify the cluster.`.dev` for **devnet** and`.prod` for **mainnet-beta**. Default is `.prod.`
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
