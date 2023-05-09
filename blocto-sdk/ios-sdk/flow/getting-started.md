---
description: Initialize FCL and add Blocto as a wallet provider
---

# Getting Started

In this guide we will show you prerequisite for Blocto Flow SDK.

A sample app is available at: [https://github.com/portto/fcl-swift/tree/main/Demo/FCL\_Cocoa\_Demo](https://github.com/portto/fcl-swift/tree/main/Demo/FCL\_Cocoa\_Demo)

## Installation

### Requirements <a href="#requirements" id="requirements"></a>

* Swift version >= 5.6
* iOS version >= 13

### CocoaPods <a href="#cocoapods" id="cocoapods"></a>

FCL-SDK is available through [CocoaPods](https://cocoapods.org/). You can include specific subspec to install, simply add the following line to your Podfile:

```ruby
pod 'FCL-SDK', '~> 0.2.0'
```

### Swift Package Manager <a href="#swift-package-manager" id="swift-package-manager"></a>

```swift
.package(url: "https://github.com/portto/fcl-swift", .upToNextMinor(from: "0.2.0"))
```

Here's an example PackageDescription:

```swift
// swift-tools-version: 5.6
import PackageDescription

let package = Package(
    name: "MyPackage",
    products: [
        .library(
            name: "MyPackage",
            targets: ["MyPackage"]
        ),
    ],
    dependencies: [
        .package(url: "https://github.com/portto/fcl-swift", .upToNextMinor(from: "0.2.0"))
    ],
    targets: [
        .target(
            name: "MyPackage",
            dependencies: [
                .product(name: "FCL_SDK", package: "fcl-swift"),
            ]
        )
    ]
)
```

## Configuration

[Register app id](../prerequest.md) (bloctoSDKAppId) in order to init `BloctoWalletProvider`

```swift
import FCL_SDK

let bloctoWalletProvider = try BloctoWalletProvider(
    bloctoAppIdentifier: bloctoSDKAppId,
    window: nil,
    network: .testnet,
    logging: true
)
fcl.config
    .put(.network(.testnet))
    .put(.supportedWalletProviders(
        [
            bloctoWalletProvider,
        ]
    ))
```

{% hint style="info" %}
If window not specify, `BloctoWalletProvider` will find top view controller from `keyWindow` to present if needed.
{% endhint %}

In order to let FCL-Swift know whether user install Blocto app, please add following scheme to your info.plist.

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-11-18 at 9.59.45 AM (1) (1) (1) (1) (3).png" alt=""><figcaption><p>info.plist</p></figcaption></figure>

If you open source code or open info.plist with external editor, it will look like this.

```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>blocto-dev</string>
    <string>blocto</string>
</array>
```

Add below code to your AppDelegate:

```swift
import UIKit
import FCL_SDK

@main
class AppDelegate: UIResponder, UIApplicationDelegate {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        return true
    }
    
    func application(
        _ app: UIApplication,
        open url: URL,
        options: [UIApplication.OpenURLOptionsKey: Any] = [:]
    ) -> Bool {
        fcl.application(open: url)
        return true
    }
    
    func application(
        _ application: UIApplication,
        continue userActivity: NSUserActivity,
        restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void
    ) -> Bool {
        fcl.continueForLinks(userActivity)
        return true
    }

}
```

Or if you using SceneDelegate:

```swift
import UIKit
import FCL_SDK

class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        if let url = connectionOptions.userActivities.first?.webpageURL {
            fcl.application(open: url)
        }
        guard let windowScene = (scene as? UIWindowScene) else { return }
        window = UIWindow(windowScene: windowScene)
        window?.rootViewController = FlowDemoViewController()
        window?.makeKeyAndVisible()
    }
    
    func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
        for context in URLContexts {
            fcl.application(open: context.url)
        }
    }
    
    func scene(_ scene: UIScene, continue userActivity: NSUserActivity) {
        fcl.continueForLinks(userActivity)
    }

}
```
