---
description: Sign and verify challenges for off-chain authentication
---

# Sign Message and Verify

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

Install from npm/yarn

```bash
$ yarn add @blocto/sdk
```

### Use web3.js

```javascript
import Web3 from "web3";
import BloctoSDK from "@blocto/sdk";

const bloctoSDK = new BloctoSDK({
  ethereum: {
    chainId: "0xa4b1", // (required) chainId to be used
    rpc: `https://rpc.ankr.com/arbitrum` // (required for Ethereum) JSON RPC endpoint
  }
});

const web3 = new Web3(bloctoSDK.ethereum);

const handleSignMessage = () => {
  web3.eth.sign(message, address);
};
```

{% embed url="https://codesandbox.io/s/blocto-sdk-evm-sign-c1c4mb?file=/src/App.js" %}

#### Verify Signature

For dApps relying on `signMessage` for off-chain authentication, Blocto follows [ERC-1271](https://eips.ethereum.org/EIPS/eip-1271) and [ERC-191](https://eips.ethereum.org/EIPS/eip-191). To verify the signature, you need to call a method on the wallet contract to check if the signature came from a rightful owner of the wallet contract.

* For single-sig wallet like MetaMask, there is only one signature returned and it's 65 bytes, e.g., `0xca5955c4098c061254ee83deda2b50ad9209beb3af41ca405578409646134bfb2963866d6d4a814e669e028c178e87c77a1aff1b39f5bac4eb84d90740e6b8511c`
* For multi-sig wallet like Blocto, there are two signatures returned and their combined size is 130 bytes, e.g., `0x0cf4603f53d0cdb797cd355d94b2d9473c5d67f93fafd99336d7525c8b6a2c262d8369949e98208433051f6386afa75d3071401a332a024fcfa418bcb9f0c6201b1bc91252375039bbd94e50c1f9caf7bf6e31dc6fadf3371592df73aeb2f4637f686ce3fc4d77c55749e9cd623eba126ea5632046d88c4286957d4d622168dda11c` from smart contract account [0xAD75944Fb47Db43dE12867eC9B48C40FEdF4d8E8](https://etherscan.io/address/0xAD75944Fb47Db43dE12867eC9B48C40FEdF4d8E8#code).

We have built the tools to carry out this verification:

* [Go implementation](https://github.com/blocto/dappauth)
* [JavaScript implementation](https://github.com/blocto/dappauth.js)

Use it in your dApps (usually on backend):

{% hint style="info" %}
Don't forget to update the RPC URL to current network.
{% endhint %}

{% tabs %}
{% tab title="Go" %}
```go
package main

import (
	"log"
	"net/http"

	"github.com/ethereum/go-ethereum/ethclient"
	"github.com/blocto/dappauth"
)

// AuthenticationHandler ..
type AuthenticationHandler struct {
	client *ethclient.Client
}

// NewAuthenticationHandler ..
func NewAuthenticationHandler(rawurl string) (*AuthenticationHandler, error) {
	client, err := ethclient.Dial(rawurl)
	if err != nil {
		return nil, err
	}
	return &AuthenticationHandler{client: client}, nil
}

// ServeHTTP serves just a single route for authentication as an example
func (a *AuthenticationHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {

	challenge := r.PostFormValue("challenge")
	signature := r.PostFormValue("signature")
	addrHex := r.PostFormValue("addrHex")

	authenticator := dappauth.NewAuthenticator(r.Context(), a.client)
	isAuthorizedSigner, err := authenticator.IsAuthorizedSigner(challenge, signature, addrHex)
	if err != nil {
		// return a 5XX status code
	}
	if !isAuthorizedSigner{
		// return a 4XX status code
	}

	// create an authenticated session for address
	// return a 2XX status code
}

func main() {
	handler, err := NewAuthenticationHandler("https://arb-mainnet-public.unifra.io")
	if err != nil {
		log.Fatal(err)
	}

	log.Fatal(http.ListenAndServe(":8080", handler))
}
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const Web3 = require('web3');
const DappAuth = require('@blocto/dappauth');

const dappAuth = new DappAuth(new Web3.providers.HttpProvider('https://arb-mainnet-public.unifra.io'));

async function debug() {
  const challenge = 'foo';
  const signature =
    '0x33838c6f4e621982c2009f9b93ecb854a4b122538159623abc87d2e4c5bd6d2e33591f443b419b3bd2790e455ba6d625f2ca14b822c5cef824ef7e9021443bed1c';
  const address = '0x86aa354fc865925f945b803ceae0b3f9d856b269';

  try {
    const isAuthorizedSigner = await dappAuth.isAuthorizedSigner(
      challenge,
      signature,
      address,
    );

    console.log(isAuthorizedSigner); // true
  } catch (e) {
    console.log(e);
  }
}
```
{% endtab %}
{% endtabs %}

{% embed url="https://codesandbox.io/s/evm-verify-the-signature-drjjnz?file=/src/App.js" %}

#### PersonalSign Technical Details

According to ERC-191 and ERC-1271, when receiving `personalSign` request with `message`, Blocto will sign:

> `0x19` + `0x0` + `[User’s wallet address]` + hash(`0x19` + `0x45 (E)` + `thereum Signed Message:\n` + `len(message)` + `message`)

#### Migration Guide for dApps which Follow the Old Protocol [ERC-1654](https://github.com/ethereum/EIPs/issues/1654)

Please check [the doc](https://portto.notion.site/Off-Chain-Signature-Verification-personalSign-Migration-Guide-509bbea098084542902554c65c6133d8).

