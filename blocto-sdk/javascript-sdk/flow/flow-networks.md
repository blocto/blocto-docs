---
description: Flow networks that Blocto supports
---

# Flow Networks

### Mainnet

* Access Node `https://flow-access-mainnet.portto.io`
* Blocto Wallet `https://flow-wallet.blocto.app/authn`

```javascript
import * as fcl from "@onflow/fcl"

fcl.config()
  .put("accessNode.api", "https://flow-access-mainnet.portto.io") // connect to Flow mainnet
  .put("challenge.handshake", "https://flow-wallet.blocto.app/authn") // use Blocto wallet
```

### Testnet 

* Access Node `https://access-testnet.onflow.org`
* Blocto Wallet `https://flow-wallet-testnet.blocto.app/authn`

```javascript
import * as fcl from "@onflow/fcl"

fcl.config()
  .put("accessNode.api", "https://access-testnet.onflow.org") // connect to Flow testnet
  .put("challenge.handshake", "https://flow-wallet-testnet.blocto.app/authn") // use Blocto testnet wallet
```

