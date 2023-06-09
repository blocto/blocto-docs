---
description: Send Transaction with Blocto wallet through Flow Client Library (FCL)
---

# Send Transaction

{% hint style="warning" %}
Before sending a transaction, user needs to connect to Blocto wallet first.
{% endhint %}

### Step 1 - Send transaction

```javascript
import * as fcl from "@blocto/fcl";

const SIMPLE_TRANSACTION = `\
transaction {
  execute {
    log("Hello World!!")
  }
}
`;


const transactionId = await fcl.mutate({
  cadence: SIMPLE_TRANSACTION,
  proposer: fcl.currentUser,
  payer: fcl.currentUser,
  limit: 50,
});

// The transactions status and events after being sealed
const transaction = await fcl.tx(transactionId).onceSealed();
```

### Step 2 - Authorizing a transaction

```javascript
import * as fcl from "@blocto/fcl";

const transactionId = await fcl.mutate({
  cadence: `
    transaction {
      prepare(acct: AuthAccount) {
        log("Hello from prepare")
      }
      execute {
        log("Hello from execute")
      }
    }
  `,
  proposer: fcl.currentUser,
  payer: fcl.currentUser,
  authorizations: [fcl.currentUser],
  limit: 50,
});

const transaction = await fcl.tx(transactionId).onceSealed();
console.log(transaction); // The transactions status and events after being sealed
```

{% embed url="https://codesandbox.io/s/flow-sign-transaction-v2-lxgc0g" %}
