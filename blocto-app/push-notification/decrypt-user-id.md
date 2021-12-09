# Decrypt User ID

Get encrypted user id from [web.js (EVM-based)](web3-provider-integration-ethereum-bsc-polygon-avalanche.md) or [web.js (Solana)](web3-provider-integration-solana.md). The user id is encrypted by AES (CBC).

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
