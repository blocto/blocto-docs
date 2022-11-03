# Prerequisite



Blocto iOS SDK supports two flows depending on whether the Blocto app is installed or not.

* **Blocto app installed**\
  ****Based on development environment, you need to download the corresponding Blocto app for testing:
  * [production](https://apps.apple.com/tw/app/blocto-%E5%8A%A0%E5%AF%86%E8%B2%A8%E5%B9%A3%E9%8C%A2%E5%8C%85-by-portto/id1481181682)
  * [staging](https://appdistribution.firebase.dev/i/8d66340bb3ad10ed) (testnet environment)
    * you can download it after we add your device to provisioning profile and release a new staging version\

* **Blocto app not installed**\
  SDK would open browser using Web SDK in your dApp to use the Blocto service. In android, Blocto SDK provider `CloseWebView` method to close web view when user click back button.

```csharp
if(Input.GetKey(KeyCode.Escape))
{
    walletProvider.CloseWebView();
}
```

You can setting `ForcedUseWebView` for use of the Web SDK instead the Blocto app.

```csharp
walletProvider.ForcedUseWebView = true;
```

## iOS Universal Links & Android Deep Link

Blocto iOS SDK uses iOS[ Universal Links](https://developer.apple.com/ios/universal-links/) or android [Deep Link](https://developer.android.com/training/app-links/deep-linking) to bring back information from Blocto wallet app to yours.\
If you do not set iOS Universal Links or android Deep Link in Developer Dashboard ([production](https://developers.blocto.app/), [staging](https://developers-staging.blocto.app/)) or any situation that can't open your app correctly.

* iOS Universal Links setting. please add Associated Domains in Xcode project setting and add new path **`/blocto`** to your apple-app-site-association

<figure><img src="../../.gitbook/assets/UniversalLink (1) (2).png" alt=""><figcaption></figcaption></figure>

* Please add a new path **`/blocto`** to your apple-app-site-association
* Android Deep Links setting, please add **`activity`** and **`intent-filter`** in yours AndroidManifest.xml

```xml
<activity android:exported="true"
          android:launchMode="singleTop"
          android:name="com.blocto.unity.CallbackActivity" 
          android:process="e.unity3d">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="blocto" />
    </intent-filter>
</activity>
```

## Unity Build Setting

In android, please add `activity` and `intent-filter` in yours AndroidManifest.xml

```xml
<activity android:name="com.blocto.unity.PluginActivity"
          android:theme="@style/UnityThemeSelector"
          android:process="e.unity3d"
          android:configChanges="keyboardHidden|orientation|screenSize"
          android:screenOrientation="portrait">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
    <meta-data android:name="unityplayer.UnityActivity" android:value="true" />
</activity>
```
