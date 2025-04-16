---
description: Flow networks that Blocto supports
---

# Flow Networks

### Mainnet

* Access Node `https://rest-mainnet.onflow.org`
* Blocto Wallet
  * front channel `https://wallet-v2.blocto.app/-/flow/authn`
  * back channel `https://wallet-v2.blocto.app/api/flow/authn`

#### Front Channel

```javascript
import * as fcl from "@blocto/fcl"

fcl.config({
  "accessNode.api": "https://rest-mainnet.onflow.org", // connect to Flow mainnet
  "discovery.wallet": `https://wallet-v2.blocto.app/-/flow/authn` // use Blocto mainnet wallet
});
```

#### Back Channel

```javascript
import * as fcl from "@blocto/fcl";

fcl.config({
  "accessNode.api": "https://rest-mainnet.onflow.org", // connect to Flow mainnet
  "discovery.wallet": "https://wallet-v2.blocto.app/api/flow/authn", // use Blocto mainnet wallet
  "discovery.wallet.method": "HTTP/POST",
});
```

### Testnet

* Access Node `https://rest-testnet.onflow.org`
* Blocto Wallet
  * front channel `https://wallet-v2-dev.blocto.app/-/flow/authn`
  * back channel `https://wallet-v2-dev.blocto.app/api/flow/authn`

#### Front Channel

```javascript
import * as fcl from "@blocto/fcl"

fcl.config({
  "accessNode.api": "https://rest-testnet.onflow.org", // connect to Flow testnet
  "discovery.wallet": `https://wallet-v2-dev.blocto.app/-/flow/authn` // use Blocto testnet wallet
});
```

#### Back Channel

```javascript
import * as fcl from "@blocto/fcl";

fcl.config({
  "accessNode.api": "https://rest-testnet.onflow.org", // connect to Flow testnet
  "discovery.wallet": "https://wallet-v2-dev.blocto.app/api/flow/authn", // use Blocto testnet wallet
  "discovery.wallet.method": "HTTP/POST",
});
```
