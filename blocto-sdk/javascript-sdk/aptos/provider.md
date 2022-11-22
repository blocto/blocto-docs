---
description: A thin JSON-RPC wrapper for interacting with chains and Blocto wallet.
---

# Provider

Blocto SDK comes with an Petra (Aptos official wallet) API-compatible  provider, you can use it to interact with Aptos network.

{% hint style="warning" %}
Note that Blocto SDK for Aptos is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

### Installation

Install from npm/yarn

```bash
$ yarn add @blocto/sdk
```

... or via CDN

```javascript
<script src="https://unpkg.com/@blocto/sdk" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
```

### Get App ID

It's required to register an app id before using Blocto SDK, check out the [Register App ID](../../register-app-id.md) section

### **Usage**

Initiate the Blocto provider&#x20;

```javascript
import BloctoSDK from '@blocto/sdk'

const bloctoSDK = new BloctoSDK({
    aptos: {
        // (required) chainId to be used
        chainId: 1,
    },
    
    // (required) Blocto app ID
    appId: 'YOUR_BLOCTO_APP_ID',
});
```

#### Blocto Provider parameters

| Parameter       | Type   | Description                  | Required |
| --------------- | ------ | ---------------------------- | -------- |
| `aptos.chainId` | number | Aptos chain ID to connect to | **Yes**  |
| `appId`         | String | Blocto dApp ID               | **Yes**  |

#### Examples

{% tabs %}
{% tab title="Aptos Mainnet" %}
```javascript
const bloctoSDK = new BloctoSDK({
    aptos: {
        chainId: 1,
    },
    appId: 'YOUR_BLOCTO_APP_ID',
});
```
{% endtab %}

{% tab title="Aptos Testnet" %}
```javascript
const bloctoSDK = new BloctoSDK({
    aptos: {
        chainId: 2,
    },
    appId: 'YOUR_BLOCTO_APP_ID',
});
```
{% endtab %}
{% endtabs %}

**Connect to Blocto wallet**\
Once the connection request is fired, there would be a prompt modal to guide user to register/login to Blocto wallet

```javascript
const {
  address, // address of the connected account
  publicKey, // public keys (array) of the multi-sig account
  authKey, // authentication key
} = await bloctoSDK.aptos.connect()
```

After connecting with Blocto wallet, you can start to interact with Aptos blockchain with Blocto provider.



