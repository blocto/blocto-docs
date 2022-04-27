---
description: Sign message with Blocto wallet through Flow Client Library (FCL)
---

# Sign Transaction

{% hint style="warning" %}
Before sending a transaction, user needs to connect to Blocto wallet first.
{% endhint %}

### Step 1 - Send transaction

```javascript
import * as fcl from "@onflow/fcl"

const SIMPLE_TRANSACTION = `\
transaction {
  execute {
    log("Hello World!!")
  }
}
`

// Get latest block info
const blockResponse = await fcl.send([
  fcl.getLatestBlock(),
])

// Decode block info
const block = await fcl.decode(blockResponse)

// Send transaction
const tx = await fcl.send([
  fcl.transaction(SIMPLE_TRANSACTION),
  fcl.proposer(fcl.currentUser().authorization),
  fcl.payer(fcl.currentUser().authorization),
  fcl.ref(block.id),
])

```

### Step 2 - Monitor transaction result

```javascript
import * as fcl from "@onflow/fcl"

// Monitor transaction result
fcl
  .tx(tx)
  .subscribe(console.log) // fires everytime tx status updates

```

