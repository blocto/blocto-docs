---
description: Switch the blockchain network to another one
---

# Switch Chain

### Introduction

It supports Ethereum/EVMs/Layer2 injected at `window.ethereum`. Blocto follows [eip-3326](https://github.com/rekmarks/EIPs/blob/3326-create/EIPS/eip-3326.md) and will not reload the webpage after switching the network.

### Usage

```javascript
// Switch to BSC
const params = [{ chainId: '0x38' }]
await window.ethereum.request({
  method: 'wallet_switchEthereumChain',
  params: params
})
```

Switching network will emit a `chainChanged` event.

```javascript
window.ethereum.on('chainChanged', (chainId) => {
  console.log(chainId) // 0x38 if it's BSC
})
```
