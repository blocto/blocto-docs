---
description: >-
  Blocto injects web3 provider to web context so dApps can interact with
  blockchain
---

# Web3.js Integration \(Solana\)

### Injection

Blocto injects web3 provider to web context as `window.bloctoProvider` and `window.solana` starting from version **2.9.0** on Android/iOS. To detect Blocto specifically, there is a `isBlocto` flag.

```javascript
console.log(window.solana.isBlocto);
// prints True
```



