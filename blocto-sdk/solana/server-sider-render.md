# Server Sider Render

Blocto SDK was designed for browser environments since it requires users to manually interact with the UI to approve transactions and signatures. Since it use some browser APIs, building app with SDK in node environment will likely fail.

However, you can use `dynamic import` with `code splitting` supported frameworks to set up Blocto SDK in the runtime:

```javascript
let bloctoSDK
import('@blocto/sdk').then(({ default: BloctoSDK }) => {
  bloctoSDK = new BloctoSDK({ solana: { net: 'mainnet-beta' } })
  bloctoSDK.soalana.connect()
})
```

Most modern code bundler like webpack support code splitting out of the box, and node supports dynamic import since version `13.2`. If you're using an older version of node, please checkout the [babel polyfill plugin](https://www.npmjs.com/package/babel-plugin-dynamic-import-node).
