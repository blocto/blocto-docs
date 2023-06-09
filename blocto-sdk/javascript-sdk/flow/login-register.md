---
description: Connect to Blocto wallet through Flow Client Library (FCL)
---

# Authenticate / Unauthenticate

### Step 1 - Configure FCL

```javascript
import * as fcl from "@blocto/fcl";

fcl
  .config()
  // connect to Flow testnet
  .put("accessNode.api", "https://rest-testnet.onflow.org")
  // use Blocto testnet wallet
  .put(
    "discovery.wallet",
    `https://wallet-v2-dev.blocto.app/${YOUR_DAPP_ID}/flow/authn`
  )
```

Alternatively, if you already have user's email and would like to pre-fill it for user's Blocto account, you can use the custom `discovery.wallet` URL instead:

```javascript
import * as fcl from "@blocto/fcl";

const USER_EMAIL = "client@email.com";

fcl
  .config()
  // connect to Flow testnet
  .put("accessNode.api", "https://rest-testnet.onflow.org")
  // use Blocto testnet wallet
  .put(
    "discovery.wallet",
    `https://wallet-v2-dev.blocto.app/${YOUR_DAPP_ID}/flow/authn/${USER_EMAIL}`
  )

```

### Step 2 - Authenticate

```javascript
import * as fcl from "@blocto/fcl";

// fires everytime account connection status updates
fcl.currentUser().subscribe(console.log);

// authenticate
fcl.authenticate();
```

### Step 3 - Unauthenticate

```javascript
import * as fcl from "@blocto/fcl";

// fires everytime account connection status updates
fcl.currentUser().subscribe(console.log);

// unauthenticate and clear account info in FCL
fcl.unauthenticate();
```

{% embed url="https://codesandbox.io/s/flow-autenticate-unauthenticate-v2-hfq56w" %}
