---
description: Switch between EVM compatible chains
---

# Switch Ethereum Chain

Start from `@blocto/sdk` version `^0.4.1`, we support two new RPC method `wallet_switchEthereumChain` and `wallet_addEthereumChain` let developers easier to switch between EVM compatible chains​.&#x20;

### wallet\_addEthereumChain[​](https://docs.metamask.io/wallet/reference/rpc-api/#wallet\_addethereumchain) <a href="#wallet_addethereumchain" id="wallet_addethereumchain"></a>

This method will add the specified chain into Blocto SDK instance.&#x20;

Blocto SDK will validate the parameters for this method, and rejects the request if any parameter is incorrectly formatted. Blocto SDK also rejects the request if any of the following occurs:

* Provided `chainId` isn't supported yet.
* The RPC endpoint isn't in our whitelist.
* The RPC endpoint doesn't respond to RPC calls.
* The RPC endpoint returns a different chain ID when `eth_chainId` is called.

This method is specified by [EIP-3085](https://eips.ethereum.org/EIPS/eip-3085).

#### **Parameters**[**​**](https://docs.metamask.io/wallet/reference/rpc-api/#parameters-1)

An array containing an object containing the following metadata about the chain to be added to Blocto SDK:

* `chainId` - The chain ID as a `0x`-prefixed hexadecimal string or number.
* `rpcUrls` - An array of RPC URL strings. At least one item is required, and only the first item is used.

Note: At least one metadata is required, and only the first metadata in array is used. If you wish to load multiple chain metadata, please check [#batch-add-ethereum-chain](switch-ethereum-chain.md#batch-add-ethereum-chain "mention") part.

**Returns**[**​**](https://docs.metamask.io/wallet/reference/rpc-api/#returns-3)

`null` if the request was successful, and an error otherwise.

#### **Example**[**​**](https://docs.metamask.io/wallet/reference/rpc-api/#example-2)

We recommend using this method before `wallet_switchEthereumChain`:

```typescript
bloctoSDK.ethereum
  .request({
    method: "wallet_addEthereumChain", // add chain first
    params: [
      {
        chainId: 97, // BSC testnet
        rpcUrls: ["https://..."],
      },
    ],
  })
  .then(() => {
    // you can switch to chain 97 now
    bloctoSDK.ethereum.request({
      method: "wallet_switchEthereumChain",
      params: [{ chainId: 97 }],
    });
  });
```

### wallet\_switchEthereumChain[​](https://docs.metamask.io/wallet/reference/rpc-api/#wallet\_switchethereumchain) <a href="#wallet_switchethereumchain" id="wallet_switchethereumchain"></a>

This method will switch to the chain with the specified chain ID.

Blocto SDK rejects the request if any of the following occurs:

* The chain ID is malformed.
* The chain with the specified chain ID hasn't been added to Blocto SDK.

This method is specified by [EIP-3326](https://ethereum-magicians.org/t/eip-3326-wallet-switchethereumchain).

#### **Parameters**[**​**](https://docs.metamask.io/wallet/reference/rpc-api/#parameters-2)

An array containing an object containing `chainId`, the chain ID as a `0x`-prefixed hexadecimal string.

#### **Returns**[**​**](https://docs.metamask.io/wallet/reference/rpc-api/#returns-4)

`null` if the request was successful, and an error otherwise.

If the error code is `4902`, the requested chain hasn't been added by Blocto SDK, and you must request to add it using `wallet_addEthereumChain`.

#### **Example**[**​**](https://docs.metamask.io/wallet/reference/rpc-api/#example-3)

See the `wallet_addEthereumChain`[#example](switch-ethereum-chain.md#example "mention").

### Batch Add Ethereum Chain

You can also add multiple params in `wallet_addEthereumChain` .

#### Example

```javascript
bloctoSDK.ethereum
  .request({
    method: "wallet_addEthereumChain", // load all chains you want to switch
    params: [
      {
        chainId: 97, // BSC testnet
        rpcUrls: ["https://..."],
      },
      {
        chainId: 80001, // Polygon testnet
        rpcUrls: ["https://..."],
      },
    ],
  })
  .then(() => {
    // you can switch to chain 97 now
    bloctoSDK.ethereum.request({
      method: "wallet_switchEthereumChain",
      params: [{ chainId: 97 }],
    });
  });
```

Then you can switch between BSC and polygon using `wallet_switchEthereumChain` method.&#x20;

```javascript
bloctoSDK.ethereum.request({
  method: "wallet_switchEthereumChain",
  params: [{ chainId: 97 }],
});
```

### Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk/tree/main/examples/with-evm-blocto-switch-chain" %}
