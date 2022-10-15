# Blocto Environment

Blocto is a cross-chain wallet and uses email as login. There are two environments in Blocto: _prod_ and _dev_ which corresponding to different blockchain networks. The same environment means the same account system.

| Chain \ Environment | prod              | dev                    | prod |
| ------------------- | ----------------- | ---------------------- | ---- |
| Ethereum            | Mainnet           | Rinkeby                |      |
| BNB Chain           | Mainnet           | Chapel Testnet         |      |
| Polygon             | Mainnet           | Mumbai                 |      |
| Avalanche           | Mainnet (C Chain) | Fuji Testnet (C Chain) |      |
| Solana              | Mainnet Beta      | Devnet                 |      |
| Flow                | Mainnet           | Testnet                |      |
| Aptos               | Mainnet           | Devnet                 |      |

{% hint style="info" %}
We're building _staging_ environment which is more stable than _dev_.
{% endhint %}



### Javascript SDK

Blocto Javascript SDK uses different environment according to blockchain network. The key properties are as below:

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
