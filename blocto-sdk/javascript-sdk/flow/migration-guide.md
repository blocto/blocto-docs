---
description: Migrating Blocto wallet from v1 to v2
---

# Migration Guide

This guide is primarily for early Flow developers adopting Blocto wallet who want to learn about our latest progress on Flow & Blocto. Read the following guide to enhance your experience with Blocto.

{% hint style="info" %}
For why to use `@blocto/fcl` instead of `@onflow/fcl`, check out this [Github PR](https://github.com/onflow/fcl-js/pull/1679) for further information.
{% endhint %}

### Changes

* Latest version of `@blocto/fcl@1.6.1` has been released
* New version of the blocto wallet interface and endpoints
* Update some deprecated methods or properties

### Upgrading Dependency

Upgrade the `@blocto/fcl` in the package.json to `^1.6.1`

{% tabs %}
{% tab title="yarn" %}
```bash
yarn upgrade @blocto/fcl@^1.6.1
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install @blocto/fcl@^1.6.1
```
{% endtab %}

{% tab title="pnpm" %}
```bash
npm install @blocto/fcl@^1.6.1
```
{% endtab %}
{% endtabs %}

### Upgrading the FCL Configuration

Replace the value of `accessNode.api` and configuration keys:



{% tabs %}
{% tab title="Mainnet" %}
```
import * as fcl from "@blocto/fcl";

fcl
  .config({
    "accessNode.api": "https://rest-mainnet.onflow.org",
    "discovery.wallet": `https://wallet-v2.blocto.app/${YOUR_DAPP_ID || "-"}/flow/authn`
  })
```
{% endtab %}

{% tab title="Testnet" %}
```
import * as fcl from "@blocto/fcl";

fcl
  .config({
    "accessNode.api": "https://rest-testnet.onflow.org",
    "discovery.wallet": `https://wallet-v2-dev.blocto.app/${YOUR_DAPP_ID || "-"}/flow/authn`
  })
```
{% endtab %}
{% endtabs %}

For dApps that haven't applied for a dApp ID, we recommend you apply for a better user experience, or use `-` as an alternative.

{% hint style="info" %}
Visit [register-app-id.md](../../register-app-id.md "mention") to learn how to register App ID
{% endhint %}

If you have been using the back channel to communicate with FCL, follow the guide below:





{% tabs %}
{% tab title="Mainnet" %}
```
import * as fcl from "@blocto/fcl";

fcl
  .config({
    "accessNode.api": "https://rest-mainnet.onflow.org",
    "discovery.wallet": "https://wallet-v2.blocto.app/api/flow/authn",
    "app.detail.id": "YOUR_DAPP_ID"// this line is optional
    "discovery.wallet.method": "HTTP/POST"
  })
```
{% endtab %}

{% tab title="Testnet" %}
```
import * as fcl from "@blocto/fcl";

fcl
  .config({
    "accessNode.api": "https://rest-testnet.onflow.org",
    "discovery.wallet": "https://wallet-v2-dev.blocto.app/api/flow/authn",
    "app.detail.id": "YOUR_DAPP_ID"// this line is optional
    "discovery.wallet.method": "HTTP/POST"
  })
```
{% endtab %}
{% endtabs %}

For dApps that haven't applied for a dApp ID, there is no need to set the `app.detail.id` key.



### FCL Wallet Discovery

Good news for those who use [wallet discovery](https://github.com/onflow/fcl-discovery) to support multiple wallet on Flow, there is no action required from your side. You can keep the configuration as you did in FCL and Blocto wallet v2 will show up there automatically.
