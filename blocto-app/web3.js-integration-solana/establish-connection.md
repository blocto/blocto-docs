---
description: Connect with Blocto from the web app.
---

# Establish Connection

Before you interact with Blocto, you have to establish a connection first. The connection will prompt the user for permission to share their public key, or it returns public key directly if the user has approved the account access permission.

## Connection

```javascript
window.solana.connect()
```

When the user has approved the request to connect, the provider will emit a `connect` event. And you may access useful properties of the provider.

```javascript
window.solana.on("connect", () => console.log("connected"))

window.solana.publicKey // wallet's publicKey
window.solana.connected // true or false
window.solana.network // mainnet-beta or testnet
```

## Disconnection

Similar with connection, calling disconnection will emit a `disconnect` event.

```javascript
window.solana.disconnect()

window.solana.on("disconnect", () => console.log("disconnected"))
```

