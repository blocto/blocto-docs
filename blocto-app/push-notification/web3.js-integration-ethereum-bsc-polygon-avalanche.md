# Web3.js Integration (Ethereum/BSC/Polygon/Avalanche)

### Getting Encrypted User IDs From your Web App

Blocto injects a global API into websites at `window.bloctoProvider` and `window.ethereum` at the same time. Currently, Blocto supports networks:

* Ethereum
* BSC
* Polygon
* Avalanche (C-Chain)

Please contact Blocto team to get your app ID.

#### Register Push Notification

```javascript
let web3 = new Web3(window.ethereum) // or window.tangerine or window.bloctoProvider
if (web3.currentProvider.isBlocto) {
  try {
    let encryptedUserId = await web3.currentProvider.request({
      method: 'blocto_registerPushNotification',
      params: ["{appId}"]
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
