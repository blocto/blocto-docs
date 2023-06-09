---
description: Flow networks that Blocto supports
---

# Flow Networks

### Mainnet

* Access Node `https://rest-mainnet.onflow.org`
* Blocto Wallet&#x20;
  * front channel `https://wallet-v2.blocto.app/${YOUR_DAPP_ID}/flow/authn`
  * back channel `https://wallet-v2.blocto.app/api/flow/authn`

#### Front Channel

```javascript
import * as fcl from "@blocto/fcl"

fcl
  .config()
  .put("accessNode.api", "https://rest-mainnet.onflow.org")// connect to Flow mainnet
  .put("discovery.wallet", `https://wallet-v2.blocto.app/${YOUR_DAPP_ID}/flow/authn`)// use Blocto wallet
```

#### Back Channel

```javascript
import * as fcl from "@blocto/fcl";

fcl
  .config()
  .put("accessNode.api", "https://rest-mainnet.onflow.org")// connect to Flow mainnet
  .put("discovery.wallet","https://wallet-v2.blocto.app/api/flow/authn")// use Blocto wallet
  .put("discovery.wallet.method", "HTTP/POST")
  .put("app.detail.id", YOUR_DAPP_ID)// this line is optional
```

### Testnet

* Access Node `https://rest-testnet.onflow.org`
* Blocto Wallet&#x20;
  * front channel `https://wallet-v2-dev.blocto.app/${YOUR_DAPP_ID}/flow/authn`
  * back channel `https://wallet-v2-dev.blocto.app/api/flow/authn`

#### Front Channel

```javascript
import * as fcl from "@blocto/fcl"

fcl
  .config()
  .put("accessNode.api", "https://rest-testnet.onflow.org")// connect to Flow testnet
  .put("discovery.wallet", `https://wallet-v2-dev.blocto.app/${YOUR_DAPP_ID}/flow/authn`)// use Blocto testnet wallet
```

#### Back Channel

```javascript
import * as fcl from "@blocto/fcl";

fcl
  .config()
  .put("accessNode.api", "https://rest-testnet.onflow.org")// connect to Flow testnet
  .put("discovery.wallet","https://wallet-v2-dev.blocto.app/api/flow/authn")// use Blocto testnet wallet
  .put("discovery.wallet.method", "HTTP/POST")
  .put("app.detail.id", YOUR_DAPP_ID)// this line is optional
```
