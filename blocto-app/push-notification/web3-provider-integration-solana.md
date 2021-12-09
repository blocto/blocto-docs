---
description: Web.js Integration (Solana)
---

# Web3.js Integration (Solana)

### Getting Encrypted User IDs From your Web App

Blocto injects a global API into websites at `window.bloctoProvider` and `window.solana` at the same time.

Please contact Blocto team to get your app ID.

#### Register Push Notification

```javascript
if (window.solana && window.solana.isBlocto) {
  try {
    let encryptedUserId = await window.solana.request({
       method: 'registerPushNotification',
       params: {
         appId: '{appId}'
       }
    })
    // send encryptedUserId to your backend
  } catch(e) {
    // error handling
    // e.mesage: see the below
  }
}
```

**Error Handling**

* `blocked`: user clicked the block button.
* `cancelled`: user clicked the cancel button.
* `noNetworkConnection`: no network connection.
* `appIdNotExist`: your app id doesn't exist.
* `internal`: wallet app internal error. Please contact us.
