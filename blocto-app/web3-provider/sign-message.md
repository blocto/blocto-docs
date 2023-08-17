---
description: Sign and verify challenges for off-chain authentication
---

# Sign Message

For dApps relying on `signMessage` for off-chain authentication, Blocto follows [ERC-1271](https://eips.ethereum.org/EIPS/eip-1271) and [ERC-191](https://eips.ethereum.org/EIPS/eip-191). To verify the signature, you need to call a method on the wallet contract to check if the signature came from a rightful owner of the wallet contract.

Blocto have built the tools to carry out this verification:

* [Go implementation](https://github.com/blocto/dappauth)
* [JavaScript implementation](https://github.com/blocto/dappauth.js)

Use it in your dApps:

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
const DappAuth = require('@blocto/dappauth');

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

#### PersonalSign Technical Details

According to ERC-191 and ERC-1271, when receiving `personalSign` request with `message`, Blocto will sign:

> `0x19` + `0x0` + `[User’s wallet address]` + hash(`0x19` + `0x45 (E)` + `thereum Signed Message:` + `len(message)` + `message`)
