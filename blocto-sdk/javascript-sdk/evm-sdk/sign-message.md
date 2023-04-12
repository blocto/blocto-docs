---
description: Sign and verify challenges for off-chain authentication
---

# Sign Message

{% hint style="warning" %}
Note that Blocto SDK for EVM-compatible chains is still in **Beta**. APIs are subject to breaking changes.
{% endhint %}

Install from npm/yarn

```bash
$ yarn add @blocto/sdk
```

### Use web3 api

```javascript
import Web3 from "web3";
import BloctoSDK from "@blocto/sdk";

const bloctoSDK = new BloctoSDK({
  ethereum: {
    chainId: "0x4", // (required) chainId to be used
    rpc: `https://rinkeby.infura.io/v3/ef5a5728e2354955b562d2ffa4ae5305` // (required for Ethereum) JSON RPC endpoint
  }
});

const web3 = new Web3(bloctoSDK.ethereum);

const handleSignMessage = () => {
  web3.eth.sign(message, address);
};
```

{% embed url="https://codesandbox.io/s/blocto-sdk-evm-sign-c1c4mb?file=/src/App.js" %}

#### Verify Signature

For dApps relying on `signMessage` for off-chain authentication, Blocto follows [EIP-1654](https://github.com/ethereum/EIPs/issues/1654). To verify the signature, you need to call a method on the wallet contract to check if the signature came from a rightful owner of the wallet contract.

Dapper Labs has built the tools to carry out this verification:

* [Go implementation](https://github.com/dapperlabs/dappauth)
* [JavaScript implementation](https://github.com/dapperlabs/dappauth.js)

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
	"github.com/dapperlabs/dappauth"
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
	handler, err := NewAuthenticationHandler("https://mainnet.infura.io")
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
const DappAuth = require('@dapperlabs/dappauth');

const dappAuth = new DappAuth(new Web3.providers.HttpProvider('http://localhost:8545'));

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
