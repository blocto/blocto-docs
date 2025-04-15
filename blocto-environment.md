# Blocto Environment

Blocto is a AA wallet and uses email as login. There are two environments in Blocto: _prod_ and _dev_ which is corresponding to different blockchain networks. The same environment means the same account system.

<table><thead><tr><th>Chain \ Environment</th><th>prod</th><th>dev</th><th data-hidden>prod</th></tr></thead><tbody><tr><td>Flow</td><td>Mainnet</td><td>Testnet</td><td></td></tr></tbody></table>

{% hint style="info" %}
We're building _staging_ environment which is more stable than _dev_.
{% endhint %}

### JavaScript SDK

Blocto JavaScript SDK uses different environments according to different blockchain networks. The key properties are as below:

* Flow: `challenge.handshake` from config

### Android App

_prod_: [Google Play](https://play.google.com/store/apps/details?id=com.portto.blocto)

dev: [Google Play](https://play.google.com/store/apps/details?id=com.portto.blocto.dev)

### iOS App

_prod_: [App Store](https://apps.apple.com/tw/app/blocto/id1481181682)

_dev_: [Firebase App Distribution](https://appdistribution.firebase.dev/i/9bf7a7686de52079) (after joining app distribution, you need to wait for releasing next version)
