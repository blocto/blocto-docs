---
description: Please upgrade your Blocto SDK / FCL ASAP
---

# Web Wallet v1 Sunset Notice and Migration Guide

2023/10/02 updated:

In order to bring the best user experience to our users, we have decided to postpone the due day of the sunset to November 6th. Please upgrade to wallet v2 as soon as possible in case there are some unexpected issues occurred.

{% hint style="info" %}
If you already used FCL wallet discovery to support multiple wallet, you can keep using the original discovery URL.
{% endhint %}



To enhance user experience and safety, we will sunset Blocto Web Wallet v1 on October 2nd, 2023. After this date, users will _no longer have access_ to Blocto Wallet on dApps integrated with Web Wallet v1. To continue providing services with Blocto Wallet, dApps need to upgrade Blocto SDK to versions interacting with v2 services.

By migrating to v2, Blocto users will enjoy an enhanced and safer experience, including new features like social login. To continue using Blocto Wallet and benefit from these features, please upgrade your Blocto SDK and/or update the FCL configuration following the guide below. The upgrade process takes approximately 15 minutes.&#x20;



### **fcl.js:**

Please upgrade fcl.js to `1.6.1` and update the configuration referring to the following migration guide: [https://docs.blocto.app/blocto-sdk/javascript-sdk/flow/migration-guide](https://docs.blocto.app/blocto-sdk/javascript-sdk/flow/migration-guide)

### **Blocto JavaScript SDK:**

Please upgrade to at least `0.4.0` (`0.7.1` is better) through [npmjs](https://www.npmjs.com/package/@blocto/sdk).

repo: [https://github.com/portto/blocto-sdk](https://github.com/portto/blocto-sdk)

### **Blocto Android SDK:**

Please upgrade to at least `0.5.1` through maven central.

repo: [https://github.com/portto/blocto-android-sdk](https://github.com/portto/blocto-android-sdk)

### **Blocto iOS SDK:**

Please upgrade to at least `0.6.1` through CocoaPods/SPM.

repo: [https://github.com/portto/blocto-ios-sdk](https://github.com/portto/blocto-ios-sdk)

### **Blocto Unity SDK:**

Please download the version accordingly from [GitHub](https://github.com/portto/blocto-unity-sdk/releases):

* Core: at least `0.2.3`
* Flow: at least `0.2.6`
* EVM: at least `0.2.0`
* Aptos: at least `0.1.1`
* Solana: at least `0.2.0`

repo: [https://github.com/portto/blocto-unity-sdk](https://github.com/portto/blocto-unity-sdk)

###

###
