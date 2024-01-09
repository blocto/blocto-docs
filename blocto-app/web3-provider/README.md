---
description: >-
  Blocto injects web3 provider to web context so dApps can interact with
  blockchain
---

# Web3.js Integration - Ethereum & EVMs & Layer2

### Injection

Blocto injects web3 provider to web context as `window.BLOCKCHAIN` where BLOCKCHAIN is either

* `ethereum`
  * support BNB Smart Chain network starting from 2.8.0 on Android/iOS
  * support Polygon/Avalanche(c-chain) network starting from 2.14.0 on Android/iOS
  * support Arbitrum/Optimism network starting from 3.22.0 on Android/iOS
  * support Scroll/Base/Zora network starting from 4.11.0 on Android/iOS

You can use the provider with your web3.js like

```javascript
import Web3 from 'web3';

// use the provider to instantiate web3 onject
const web3 = new Web3(window.ethereum);

console.log(window.ethereum.isBlocto);
// prints True
```

You can use all the web3.js functionalities and Blocto will handle all the wallet operations and blockchain interactions for you.

For more documentation for what you can do with web3.js, check out the [documentation](https://docs.web3js.org/).
