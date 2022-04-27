# Prerequest

There are two paths for using Blocto service through iOS SDK

1. User has already install Blocto native wallet app.
2. User not install Blocto (SDK will route to Web SDK for you).

{% hint style="info" %}
Based on development environment, you need to download the corresponding Blocto app for testing:

* [production](https://apps.apple.com/tw/app/blocto-%E5%8A%A0%E5%AF%86%E8%B2%A8%E5%B9%A3%E9%8C%A2%E5%8C%85-by-portto/id1481181682)
* [firebase staging](https://appdistribution.firebase.dev/i/8d66340bb3ad10ed) (you can download it after we add your device to provisioning profile and release a new staging version)
{% endhint %}

## Universal Links & Custom URL Scheme

Blocto iOS SDK uses[ Universal Links](https://developer.apple.com/ios/universal-links/) to bring back information from Blocto Wallet app to yours.\
If you not set Universal Links in Developer Dashboard ([production](https://developers.blocto.app), [staging](https://developers-staging.blocto.app)) or any situation that can't open your app correctly, we will use [Custom URL Scheme](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app) instead.&#x20;

Please make sure to implement **BOTH** [Apple Universal Links](https://developer.apple.com/ios/universal-links/) and [Custom URL Scheme](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app) for better user experience.

{% hint style="warning" %}
For iOS App we highly recommend you provider a Universal Links rather than only use custom scheme for safety reason.

You can use tools such as [ngrok](https://ngrok.com) for Universal Links on testing purpose.
{% endhint %}
