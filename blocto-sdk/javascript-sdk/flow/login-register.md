---
description: Connect to Blocto wallet through Flow Client Library (FCL)
---

# Login / Register

### Step 1 - Configure FCL

```javascript
import * as fcl from "@onflow/fcl"

fcl.config()
  // connect to Flow testnet
  .put("accessNode.api", "https://access-testnet.onflow.org")
  // use Blocto testnet wallet
  .put("challenge.handshake", "https://flow-wallet-testnet.blocto.app/authn")
```

Alternatively, if you already have user's email and would like to pre-fill it for user's Blocto account, you can use the custom handshake URL instead:

```javascript
import * as fcl from "@onflow/fcl"

fcl.config()
  // connect to Flow testnet
  .put("accessNode.api", "https://access-testnet.onflow.org")
  // use Blocto testnet wallet
  .put(
    "challenge.handshake",
    "https://flow-wallet-testnet.blocto.app/authn/-/user@some.where"
  )
```

Starting from `@onflow/fcl@0.0.77` you can also use HTTP/POST to initiate login requests, instead of iframe.  Simply modify your wallet connection to:

```javascript
import * as fcl from "@onflow/fcl"

fcl.config()
  // connect to Flow testnet
  .put("accessNode.api", "https://access-testnet.onflow.org")
  // use Blocto testnet wallet with HTTP/POST
  .put(
    "discovery.wallet",
    "https://flow-wallet-testnet.blocto.app/api/flow/authn"
  )
  .put("discovery.wallet.method", "HTTP/POST")
```

### Step 2 - Authenticate

```javascript
import * as fcl from "@onflow/fcl"

fcl
  .currentUser()
  .subscribe(console.log) // fires everytime account connection status updates
  
// authenticate
fcl.authenticate()
```

### Step 3 - Unauthenticate

```javascript
import * as fcl from "@onflow/fcl"

fcl
  .currentUser()
  .subscribe(console.log) // fires everytime account connection status updates
  
// unauthenticate and clear account info in FCL
fcl.unauthenticate()
```

