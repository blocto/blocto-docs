# Prerequisite

Blocto iOS SDK supports two flows depending on whether the Blocto app is installed or not.

* **Blocto app installed**\
  ****Based on development environment, you need to download the corresponding Blocto app for testing:
  * [prod](https://apps.apple.com/tw/app/blocto-%E5%8A%A0%E5%AF%86%E8%B2%A8%E5%B9%A3%E9%8C%A2%E5%8C%85-by-portto/id1481181682)
  * [dev](https://appdistribution.firebase.dev/i/50335e7876650bce) (after joining app distribution, you need to wait for releasing next version)\

* **Blocto app not installed**\
  SDK would open browser using Web SDK in your dApp to use the Blocto service.

## Universal Links & Custom URL Scheme

Blocto iOS SDK uses[ Universal Links](https://developer.apple.com/ios/universal-links/) to bring back information from Blocto wallet app to yours.\
If you do not set Universal Links in Developer Dashboard ([production](https://developers.blocto.app/), [staging](https://developers-staging.blocto.app/)) or any situation that can't open your app correctly, we will use [Custom URL Scheme](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app) instead.&#x20;

Please make sure to implement **BOTH** [Apple Universal Links](https://developer.apple.com/ios/universal-links/) and [Custom URL Scheme](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app) for better user experience.

{% hint style="info" %}
For custom URL Scheme please register scheme using prefix **blocto** with app id from Developer Dashboard e.g. blocto64776cec-5953-4a58-8025-772f55a3917b or blocto-staging64776cec-5953-4a58-8025-772f55a3917b
{% endhint %}

![Custom URL Scheme example](<../../.gitbook/assets/image (1).png>)

{% hint style="warning" %}
We highly recommend you provide a Universal Links rather than only use custom scheme for safety reason.

You can use tools such as [ngrok](https://ngrok.com/) for Universal Links on testing purpose.
{% endhint %}

![Universal Links example applinks:04b2-61-216-44-25.ngrok.io?mode=developer](<../../.gitbook/assets/UniversalLink (1).png>)
