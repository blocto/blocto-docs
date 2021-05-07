---
description: >-
  Blocto injects web3 provider to web context so dApps can interact with
  blockchain
---

# Web3.js Integration \(Solana, Coming Soon\)

### Injection

Blocto injects web3 provider to web context as `window.bloctoProvider` and `window.solana`. To detect Blocto specifically, there is a `isBlocto` flag.

```javascript
console.log(window.solana.isBlocto);
// prints True
```

