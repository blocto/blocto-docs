# Blocto Environment

Blocto is a cross-chain wallet and uses email as login. There are two environments in Blocto: _prod_ and _dev_ which is corresponding to different blockchain networks. The same environment means the same account system.

<table><thead><tr><th>Chain \ Environment</th><th>prod</th><th>dev</th><th data-hidden>prod</th></tr></thead><tbody><tr><td>Ethereum</td><td>Mainnet</td><td>Rinkeby</td><td></td></tr><tr><td>BNB Chain</td><td>Mainnet</td><td>Chapel Testnet</td><td></td></tr><tr><td>Polygon</td><td>Mainnet</td><td>Mumbai</td><td></td></tr><tr><td>Avalanche</td><td>Mainnet (C Chain)</td><td>Fuji Testnet (C Chain)</td><td></td></tr><tr><td>Solana</td><td>Mainnet Beta</td><td>Devnet</td><td></td></tr><tr><td>Flow</td><td>Mainnet</td><td>Testnet</td><td></td></tr><tr><td>Aptos</td><td>Mainnet</td><td>Testnet</td><td></td></tr></tbody></table>

{% hint style="info" %}
We're building _staging_ environment which is more stable than _dev_.
{% endhint %}



### Javascript SDK

Blocto Javascript SDK uses different environments according to different blockchain networks. The key properties are as below:

* EVM-based blockchains: chainId
* Solana: network
* Flow: `challenge.handshake` from config

For instance, if you specify `chainId = 0x13881` for Polygon, it will use Blocto _dev_ environment.



### Android App

_prod_: [Google Play](https://play.google.com/store/apps/details?id=com.portto.blocto)

dev: [Google Play](https://play.google.com/store/apps/details?id=com.portto.blocto.dev)



### iOS App

_prod_: [App Store](https://apps.apple.com/tw/app/blocto/id1481181682)

_dev_: [Firebase App Distribution](https://appdistribution.firebase.dev/i/9bf7a7686de52079) (after joining app distribution, you need to wait for releasing next version)
