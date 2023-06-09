---
description: Prove account ownership of a Flow account
---

# Account Proof

Flow provides a way for dApps to ask for users' signatures when they connect a wallet and verify account ownership. For more information, refer to [Account Proof on Flow](https://docs.onflow.org/fcl/reference/proving-authentication/).

Currently Blocto self-custody accounts generate this signature slightly differently and needs an alternative way to verify ownership.

```javascript
import { AppUtils } from '@blocto/fcl'

const isSignatureValid = await AppUtils.verifyAccountProof(
  appIdentifier,
  {
    address,
    nonce,
    signatures,
  },
  {
    fclCryptoContract: BLOCTO_FCLCRYPTO_CONTRACT_ADDRESS
  }
)
```

The addresses of the Blocto FCLCrypto contract `BLOCTO_FCLCRYPTO_CONTRACT_ADDRESS` can be found here:

| Network | Blocto FCLCrypto Address |
| ------- | ------------------------ |
| Mainnet | 0xdb6b70764af4ff68       |
| Testnet | 0x5b250a8a85b44a67       |

[More about verifyAccountProof api](https://github.com/onflow/fcl-js/blob/master/docs/reference/proving-authentication.mdx)

{% embed url="https://codesandbox.io/s/flow-account-proof-v2-u3twk1" %}
