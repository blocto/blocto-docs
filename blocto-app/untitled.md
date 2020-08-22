---
description: >-
  Push notification is a powerful tool most mobile web apps are missing. Blocto
  helps you bridge the gap and helps you push on demand.
---

# Push Notification

### Domains

| Environment | Domain |
| :--- | :--- |
| Staging | api-staging.blocto.app |
| Production | api.blocto.app |

### Header

```http
Authorization: <JWT>
Content-Type: application/json
```

### APIs

{% api-method method="post" host="https://api.blocto.app" path="/notification/send" %}
{% api-method-summary %}
Send push notification
{% endapi-method-summary %}

{% api-method-description %}
Send push notification to users by either user IDs or tags.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="text" type="string" required=true %}
the message shown on the push notification
{% endapi-method-parameter %}

{% api-method-parameter name="url" type="string" required=true %}
the URL to direct users to 
{% endapi-method-parameter %}

{% api-method-parameter name="tags" type="array" required=false %}
user tags to send push notification to
{% endapi-method-parameter %}

{% api-method-parameter type="array" name="user\_ids" %}
user IDs to send push notification to
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  'message_code': ok
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.blocto.app" path="/notification/tag" %}
{% api-method-summary %}
Tag users by user ID
{% endapi-method-summary %}

{% api-method-description %}
Set tag to a group of users for future push.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="tag" type="string" required=true %}
user tag
{% endapi-method-parameter %}

{% api-method-parameter name="user\_ids" type="array" required=true %}
user IDs to set tag
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  'message_code': ok
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Getting User IDs From your Web App

Blocto injects a global API into websites at `window.bloctoProvider` and `window.{network}` at the same time. Currently, Blocto supports networks:

* `ethereum`
* `tron`
* `tangerine`

#### Register Push Notification

```javascript
let web3 = new Web3(window.ethereum) // or window.tangerine or window.bloctoProvider
if (web3.currentProvider.isBlocto) {
  try {
    let encryptedUserId = await web3.currentProvider.registerPushNotification("{appId}")
    // send encryptedUserId to your backend
  } catch(e) {
    // error handling
    // e.mesage: see the below
  }
}
```

**Error Handling**

* `blocked`: user clicked the block button.
* `cancelled`: user clicked the cancel button.
* `noNetworkConnection`: no network connection.
* `appIdNotExist`: your app id doesn't exist.
* `internal`: wallet app internal error. Please contact us.

**Decrypt User ID**

{% tabs %}
{% tab title="JavaScript" %}
```javascript
var aesjs = require('aes-js');

// The aes key provided by blocto, and make sure it is stored in somewhere safe.
var key = aesjs.utils.hex.toBytes('71500f4803c54fcf9445a09e3434afce7552d9916bce4480e54c48a246be3f05');

// The encrypted data from blocto web3 provider.
var ivAndEncryptedData = aesjs.utils.hex.toBytes('3b28cd5be9629b777392e968fef4b0e9ea2715b4f9966ddbfa4dab41b6468a0c30476c6d1e4f58d1b22c9877eaf8bf626dc4f3f10932208684235927b28a2cd85d5aea5595fcd0618019fe80a1afd35c');

var iv = ivAndEncryptedData.slice(0, 16);
var encryptedBytes = ivAndEncryptedData.slice(16);

var aesCbc = new aesjs.ModeOfOperation.cbc(key, iv);
var paddedDecryptedBytes = aesCbc.decrypt(encryptedBytes);

let res = unpad(paddedDecryptedBytes);
if (res.err) {
    throw res.err
}

console.log(JSON.parse(ab2str(res.unpaddedBytes)));
// `{ user_id: '79efdb10-64dd-436a-9ec9-6bfda8c36e1d' }` is expected
// Store the `user_id` and make some mapping according to your application. 

function ab2str(buf) {
    return String.fromCharCode.apply(null, new Uint16Array(buf));
}

function unpad(decryptedBytes) {
    let paddedLen = decryptedBytes.length;
    if (paddedLen === 0) {
        return {
            unpaddedBytes: null,
            err: new Error('invalid padding size'),
        }
    }

    let padLen = decryptedBytes[paddedLen - 1];
    if (padLen > paddedLen || padLen > 16) {
        return {
            unpaddedBytes: null,
            err: new Error('invalid padding size'),
        }
    }

    for (let i = paddedLen - padLen; i < decryptedBytes.length; i++) {
        if (decryptedBytes[i] !== padLen) {
            return {
                unpaddedBytes: null,
                err: new Error('invalid padding'),
            }
        }
    }

    return {
        unpaddedBytes: decryptedBytes.slice(0, paddedLen - padLen),
        err: null,
    }
}
```
{% endtab %}

{% tab title="Golang" %}
```go
package main

import (
	"crypto/aes"
	"crypto/cipher"
	"encoding/hex"
	"encoding/json"
	"errors"
	"fmt"
)

func main() {
	// The aes key provided by blocto, and make sure it is stored in somewhere safe.
	key, err := hex.DecodeString("71500f4803c54fcf9445a09e3434afce7552d9916bce4480e54c48a246be3f05")
	if err != nil {
		panic(err)
	}

	// The encrypted data from blocto web3 provider.
	encryptedData, err := hex.DecodeString("3b28cd5be9629b777392e968fef4b0e9ea2715b4f9966ddbfa4dab41b6468a0c30476c6d1e4f58d1b22c9877eaf8bf626dc4f3f10932208684235927b28a2cd85d5aea5595fcd0618019fe80a1afd35c")
	if err != nil {
		panic(err)
	}

	decryptedData, err := CBCDecrypt(key, encryptedData)
	if err != nil {
		panic(err)
	}

	type data struct {
		UserID string `json:"user_id"`
	}
  
	d := &data{}
	err = json.Unmarshal(decryptedData, d)
	if err != nil {
		panic(err)
	}

	fmt.Println(d.UserID)
	// `79efdb10-64dd-436a-9ec9-6bfda8c36e1d` is expected
	// Store the `user_id` and make some mapping according to your application.
}

// Key stands for a AES key.
type Key []byte

func CBCDecrypt(key Key, cipherBytes []byte) ([]byte, error) {
	block, err := aes.NewCipher(key)
	if err != nil {
		return nil, fmt.Errorf("failed to create new aes cipher with key [%s]. err(%s)",
			key, err)
	}

	if len(cipherBytes) < aes.BlockSize {
		return nil, errors.New("cipher bytes block size is too short")
	}

	// Use the beginning of the cipher as the initialization vector.
	iv := cipherBytes[:aes.BlockSize]
	cipherBytes = cipherBytes[aes.BlockSize:]

	// Decrypt data in CBC mode.
	mode := cipher.NewCBCDecrypter(block, iv)
	decrypted := make([]byte, len(cipherBytes))
	mode.CryptBlocks(decrypted, cipherBytes)

	unpadded, err := unpad(decrypted)
	if err != nil {
		return nil, fmt.Errorf("failed to unpad data. err(%s)", err)
	}

	return unpadded, nil
}

// unpad removes the padding of a given byte array, according to the same rules
// as described in the pad function.
func unpad(padded []byte) ([]byte, error) {
	paddedLen := len(padded)
	if paddedLen == 0 {
		return nil, errors.New("invalid padding size")
	}

	pad := padded[paddedLen-1]
	padLen := int(pad)
	if padLen > paddedLen || padLen > aes.BlockSize {
		return nil, errors.New("invalid padding size")
	}

	for _, v := range padded[paddedLen-padLen : paddedLen-1] {
		if v != pad {
			return nil, errors.New("invalid padding")
		}
	}

	return padded[:paddedLen-padLen], nil
}
```
{% endtab %}
{% endtabs %}

