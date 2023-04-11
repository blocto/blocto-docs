---
description: Prove account ownership of a Flow account
---

# Account Proof

Flow provides a way for dapps to ask for users' signatures when they connect a wallet and verify account ownership. For more information, refer to [Account Proof on Flow](https://docs.onflow.org/fcl/reference/proving-authentication/).

Currently Blocto self-custody accounts generate this signature slightly differently and needs an alternative way to verify ownership.

```javascript
import { AppUtils } from '@onflow/fcl'

// as of fcl@^1.0.0
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

// as of fcl@0.0.79
const isSignatureValid = const isValid = await fcl.verifyUserSignatures(
  message,
  signatures,
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

## as of fcl@^1.0.0

{% embed url="https://codesandbox.io/s/blocto-fcl-proofaccount-6cdr4h?file=/src/App.js" %}
