---
description: Migrating Blocto wallet from v1 to v2
---

# Migration Guide

This guide is primarily for early Flow developers adopting Blocto wallet who want to learn about our latest progress on Flow & Blocto. Read the following guide to enhance your experience with Blocto.

{% hint style="info" %}
For why to use `@blocto/fcl` instead of `@onflow/fcl`, check out this [Github PR](https://github.com/onflow/fcl-js/pull/1679) for further information.
{% endhint %}

### Changes

* Latest version of `@blocto/fcl@1.4.0` has been released
* New version of the blocto wallet interface and endpoints
* Update some deprecated methods or properties

### Upgrading Dependency

Upgrade the `@blocto/fcl` in the package.json to `^1.4.0`

{% tabs %}
{% tab title="yarn" %}
```bash
yarn upgrade @blocto/fcl@^1.4.0
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install @blocto/fcl@^1.4.0
```
{% endtab %}

{% tab title="pnpm" %}
```bash
npm install @blocto/fcl@^1.4.0
```
{% endtab %}
{% endtabs %}

### Upgrading the FCL Configuration

Replace the value of `accessNode.api` and configuration keys:

```git
import * as fcl from "@blocto/fcl";


fcl
  .config()
- .put("accessNode.api", "https://access-mainnet.onflow.org")
+ .put("accessNode.api", "https://rest-mainnet.onflow.org")
- .put("challenge.handshake", "https://flow-wallet.blocto.app/authn")
+ .put("discovery.wallet",`https://wallet-v2.blocto.app/${YOUR_DAPP_ID || "-"}/flow/authn`)
```

For dApps that haven't applied for a dApp ID, we recommend you apply for a better user experience, or use `-` as an alternative.

{% hint style="info" %}
Visit [register-app-id.md](../../register-app-id.md "mention") to learn how to register App ID
{% endhint %}

If you have been using the back channel to communicate with FCL, follow the guide below:

```git
import * as fcl from "@blocto/fcl";

fcl.config()
- .put("accessNode.api", "https://access-mainnet.onflow.org")
+ .put("accessNode.api", "https://rest-mainnet.onflow.org")
- .put("discovery.wallet", "https://flow-wallet.blocto.app/api/flow/authn")
+ .put("discovery.wallet",`https://wallet-v2.blocto.app/api/flow/authn`)
+ .put("app.detail.id", YOUR_DAPP_ID)// this line is optional
  .put("discovery.wallet.method", "HTTP/POST")
```

For dApps that haven't applied for a dApp ID, there is no need to set the `app.detail.id` key.
