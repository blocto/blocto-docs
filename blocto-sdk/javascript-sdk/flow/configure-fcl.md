---
description: Configure Blocto wallet to your Flow dApp
---

# Configure FCL

### Installation

{% hint style="info" %}
FCL is under heavy developments and the versions are not always backward compatible. We recommend that you use `@`blocto`/fcl@^1.4.0` for now.
{% endhint %}

{% hint style="info" %}
For why to use `@blocto/fcl` instead of `@onflow/fcl`, check out this [Github PR](https://github.com/onflow/fcl-js/pull/1679) for further information.
{% endhint %}

{% tabs %}
{% tab title="npm" %}
```bash
npm install @blocto/fcl@^1.4.0
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @blocto/fcl@^1.4.0
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm install @blocto/fcl@^1.4.0
```
{% endtab %}
{% endtabs %}

### **Configure Flow Networks & Blocto wallet URL**

Blocto wallet provides two ways to communicate with FCL, according to your needs, choose one of them to configure your dApp. Visit [How to Configure FCL](https://developers.flow.com/tooling/fcl-js/configure-fcl) to get further configuration settings.

#### Using front channel

{% tabs %}
{% tab title="Mainnet" %}
```javascript
import * as fcl from "@blocto/fcl";
  
fcl.config({
  "accessNode.api": "https://rest-mainnet.onflow.org", // connect to Flow mainnet
  "discovery.wallet": `https://wallet-v2.blocto.app/${YOUR_DAPP_ID}/flow/authn` // use Blocto mainnet wallet
});
```
{% endtab %}

{% tab title="Testnet" %}
{% code fullWidth="false" %}
```javascript
import * as fcl from "@blocto/fcl";

fcl.config({
  "accessNode.api": "https://rest-testnet.onflow.org", // connect to Flow testnet
  "discovery.wallet": `https://wallet-v2-dev.blocto.app/${YOUR_DAPP_ID}/flow/authn` // use Blocto testnet wallet
});
```
{% endcode %}
{% endtab %}
{% endtabs %}

#### Using back channel

{% tabs %}
{% tab title="Mainnet" %}
```javascript
import * as fcl from "@blocto/fcl";

fcl.config({
  "accessNode.api": "https://rest-mainnet.onflow.org", // connect to Flow mainnet
  "discovery.wallet": "https://wallet-v2.blocto.app/api/flow/authn", // use Blocto mainnet wallet
  "discovery.wallet.method": "HTTP/POST",
  "app.detail.id": "YOUR_DAPP_ID"
});
```
{% endtab %}

{% tab title="Testnet" %}
{% code fullWidth="false" %}
```javascript
import * as fcl from "@blocto/fcl";
  
fcl.config({
  "accessNode.api": "https://rest-testnet.onflow.org", // connect to Flow testnet
  "discovery.wallet": "https://wallet-v2-dev.blocto.app/api/flow/authn", // use Blocto testnet wallet
  "discovery.wallet.method": "HTTP/POST",
  "app.detail.id": "YOUR_DAPP_ID"
});
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Visit [register-app-id.md](../../register-app-id.md "mention") to learn how to register App ID
{% endhint %}
