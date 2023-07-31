---
description: A quick overview how to use ERC-4337 easily with Blocto SDK
---

# Send ERC-4337 UserOperation

## Introduction to ERC-4337

ERC-4337 is an Ethereum standard that achieves account abstraction on the protocol without any consensus-layer changes. ERC-4337 makes it possible to transact and create contracts in a single contract account. It has four main parts: UserOperation, Bundler, EntryPoint, and Contract Account. There are also optional components called Paymasters and Aggregators that can be added. Before we start, let's take a look with these concepts.

{% hint style="info" %}
If you already familiar with ERC-4337, you can jump to next section [#how-to-use-erc-4337-with-blocto](send-erc-4337-useroperation.md#how-to-use-erc-4337-with-blocto "mention")
{% endhint %}

#### UserOperations

UserOperations are like pretend transactions that help you perform actions with contract accounts. They are made by your app.

#### Bundlers

Bundlers are like messengers who gather UserOperations from a mempool and send them to the EntryPoint contract on the blockchain.

#### EntryPoint

EntryPoint is a special smart contract that takes care of checking and carrying out the instructions in the transactions.

#### Contract Accounts

Contract Accounts are special accounts owned by users that use smart contracts.

#### Paymasters and Aggregators

Paymasters are optional accounts that can help sponsor transactions for Contract Accounts.

Aggregators are also optional accounts that can check the signatures for multiple Contract Accounts at once.

## How to use ERC-4337 with Blocto

Although we mention many complicated details with ERC-4337 above, sending ERC-4337 UserOperation is extremely easy with Blocto. We have already done most of works so you can start sending your first `UserOperation` within 5 mins.

{% hint style="warning" %}
Only support the accounts which create EVM wallets after July 10, 2023.
{% endhint %}

#### EIP-4337 Supported Chain List

<table><thead><tr><th width="395">Network</th><th width="172.33333333333331">Chain ID</th><th>EIP-4337 Support</th></tr></thead><tbody><tr><td>Arbitrum Mainnet</td><td>42161</td><td>Yes</td></tr><tr><td>Arbitrum Goerli Testnet</td><td>421613</td><td>Yes</td></tr><tr><td>Polygon Mainnet</td><td>137</td><td>Yes</td></tr><tr><td>Polygon Mumbai Testnet</td><td>80001</td><td>Yes</td></tr></tbody></table>

### Sending like Regular Transaction

Here comes the magic: since Blocto is already a contract wallet, the Blocto SDK will **automatically** transform your transaction request to an **ERC-4337 UserOperation** ready transaction if supported. Users can choose from ERC-4337 way or traditional transaction way. That means after integrate with Blocto SDK, you are already support the "4337 mode" 🎉

To know more about how the magic works, let's start from how regular transactions work in an EVM network. A transaction typically consists of three main parts: `to`, `value`, and an optional `data` field.

To understand this, let's look at a simple example of sending 1 ETH (the native token) from account A to account B. In this case, the transaction will contain the following information:

* `To: Address of account B` This is the address of account B.
* `Value: 1 ETH` It will be 1 ETH, indicating the amount being sent.
* `Data: null` Since it's a straightforward ETH transfer, the data field will be empty or null.

Now, let's consider a different scenario where account A wants to send 100 USDC (an ERC-20 token) to account B. ERC-20 tokens are essentially smart contracts that keep track of balances for different addresses. In this case, the transaction would look like this:

* `To: Address of USDC contract` Since you want to interact with USDC contract, this will be the address of the USDC smart contract.
* `Value: 0 ETH` Since we're dealing with a contract interaction, the value will be 0 ETH. The transfer of value is represented through the `Data` param send to token contract.
* `Data: Instructions to transfer 100 USDC from account A to account B` The data field will contain instructions specifying the transfer of 100 USDC from account A to account B.

The **`data`** field is **how we send instructions to a smart contract.** The `callData` in an ERC-4337 `UserOperation` is no different: It's also the instructions send to the `sender` smart contract address. That's why we can transform regular transaction to UserOperation.

### Sending UserOperation

{% hint style="info" %}
Only support with Blocto JavaScript SDK version`^0.5.0`.
{% endhint %}

All components of ERC-4337 revolve around a pseudo-transaction object called a UserOperation which is used to execute actions through a smart contract account. It captures the intent of what the user wants to do. This isn't to be mistaken for a regular transaction type.

<table><thead><tr><th>Field</th><th width="125.33333333333331">Type</th><th width="306">Description</th><th>Required</th></tr></thead><tbody><tr><td><code>callData</code></td><td><code>bytes</code></td><td>Data that's passed to the <code>sender</code> for execution.</td><td>true</td></tr><tr><td><code>sender</code></td><td><code>address</code></td><td>The address of the smart contract account to send UserOperation. If provided, should be same as login address.</td><td>false</td></tr><tr><td><code>nonce</code></td><td><code>uint256</code></td><td>Anti-replay protection. <br><strong>No need to provide</strong> since Blocto will handle it for you.</td><td>false<br>(Will be Ignored)</td></tr><tr><td><code>initCode</code></td><td><code>bytes</code></td><td>Code used to deploy the account if not yet on-chain.<br><strong>No need to provide</strong> since Blocto will handle it for you.</td><td>false<br>(Will be Ignored)</td></tr><tr><td><code>callGasLimit</code></td><td><code>uint256</code></td><td>Gas limit for the execution phase.</td><td>false</td></tr><tr><td><code>verificationGasLimit</code></td><td><code>uint256</code></td><td>Gas limit for the verification phase.</td><td>false</td></tr><tr><td><code>preVerificationGas</code></td><td><code>uint256</code></td><td>Gas to compensate the bundler for the overhead to submit a UserOperation.</td><td>false</td></tr><tr><td><code>maxFeePerGas</code></td><td><code>uint256</code></td><td>Similar to EIP-1559 max fee.</td><td>false</td></tr><tr><td><code>maxPriorityFeePerGas</code></td><td><code>uint256</code></td><td>Similar to EIP-1559 priority fee.</td><td>false</td></tr><tr><td><code>paymasterAndData</code></td><td><code>bytes</code></td><td>Paymaster contract address and any extra data the paymaster contract needs for verification and execution. When set to <code>0x</code> or the zero address, no paymaster is used.</td><td>false</td></tr><tr><td><code>signature</code></td><td><code>bytes</code></td><td>Used to validate a UserOperation during verification.<br><strong>No need to provide</strong> since Blocto will handle it for you.</td><td>false<br>(Will be Ignored)</td></tr></tbody></table>

#### How to Encode `callData`&#x20;

You might find that `callData` is the only required field of UserOperation Object to send with Blocto. The `callData` is the instructions for smart contract and should be encoded as `bytesLike` string.

Luckily, we have some helpful tools available to make the encoding process easier. One such tool is `ethers.js`. All we need is the contract's [Application Binary Interface (ABI)](https://docs.ethers.io/v5/api/utils/abi/#application-binary-interface) to work with it. The ABI provides ethers.js with the necessary information for encoding and decoding.

When using ethers.js, you have two options for the ABI: a [human-readable ABI](https://docs.ethers.io/v5/api/utils/abi/formats/#abi-formats--human-readable-abi) or a [solidity JSON ABI](https://docs.ethers.io/v5/api/utils/abi/formats/#abi-formats--solidity). If you have the Solidity compiler, you can export the JSON ABI from there. Both types of ABI can be used with ethers.js to simplify the encoding process.

The human-readable ABI of Blocto Account Contract provided here:

```typescript
// ABI of Blocto Account Contract
const bloctoAccountABI = [
  'function execute(address dest, uint256 value, bytes func)',
  'function executeBatch(address[] dest, uint256[] value, bytes[] func)',
];
```

And we assume an ERC-20 token contract has human-readable ABI look like this:

```typescript
// Sample ABI of ERC20 contract you want to interact
const partialERC20TokenABI = [
  "function transfer(address to, uint amount) returns (bool)",
];
```

{% hint style="info" %}
An ABI can be fragments and does not have to include the entire interface. That means you can includes only the parts you want to use.
{% endhint %}

With these two ABIs we can encode a `callData` for our UserOperation Object that sends an `amount` of an ERC-20 token to another account's address:

```typescript
const accountContract = new ethers.utils.Interface(bloctoAccountABI);
const erc20Token = new ethers.utils.Interface(partialERC20TokenABI);

const callData = accountContract.encodeFunctionData("execute", [
  erc20TokenContractAddress,
  ethers.constants.Zero,
  erc20Token.encodeFunctionData("transfer", [accountAddress, amount]),
]);
```

{% hint style="info" %}
#### Why are there two ABIs?

The Blocto account is a **Smart Contract Wallet**. In example above, we use our Smart Contract Wallet to interact with the **ERC-20 smart contract**. Which is why we need encoding the data twice within the `callData`.
{% endhint %}

If we only wanted to send an `amount` of ETH (the native token) to another account's address, the code would look like this:

```typescript
const accountContract = new ethers.utils.Interface(bloctoAccountABI);

const callData = accountContract.encodeFunctionData("execute", [accountAddress, amount]);
```

Just like the simplest regular transaction, there is no data field. Which means we **don't** need to encode any data within the userOp's `callData`.

#### Submit UserOperation

After you get encoded callData, you can submit your UserOperation using JSON-RPC API:

```typescript
const userOpHash = await bloctoSDK.ethereum
  .request({
    method: 'eth_sendUserOperation',
    params: [{ callData }],
  })
```

Or using type-safe `sendUserOperation` function provided:

```typescript
const userOpHash = await bloctoSDK.ethereum.sendUserOperation({ callData })
```

{% hint style="warning" %}
**Don't forget to add error handling.** Sending user operation may fail due to user using old version of account contract, target EVM chain unsupport or some other reason.
{% endhint %}

## Other RPC Methods of Blocto ERC-4337 bundler

After submit UserOperation succesfully, the next step is deal with the `userOpHash` you get to see if success or not. Here's some related method provided by Blocto can help you.

#### Get UserOperation receipt

Fetches the UserOperation receipt based on a given `userOpHash` returned from `eth_sendUserOperation`.

{% code title="Request" %}
```typescript
const result = await bloctoSDK.ethereum
  .request({
    method: 'eth_getUserOperationReceipt',
    params: [userOpHash],
  })
```
{% endcode %}

{% code title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getUserOperationReceipt",
  "params": [
    // The hash of the UserOperation
    userOpHash,
    // The EntryPoint address
    entryPoint,
    // The contract account address
    sender,
    // nonce of the UserOperation
    nonce,
    // The paymaster address
    paymaster,
    // The actual amount paid for this UserOperation
    actualGasCost,
    // The total gas used by this UserOperation
    actualGasUsed,
    // Boolean value indicating if the execution completed without revert
    success,
    // If revert occurred, this is the reason
    reason,
    // logs generated by this UserOperation only
    logs,
    // The TransactionReceipt object for the entire bundle.
    receipt
  ]
}
```
{% endcode %}

#### Get UserOperation by hash

Fetches the UserOperation and transaction context based on a given `userOpHash` returned from `eth_sendUserOperation`.

{% code title="Request" %}
```typescript
const result = await bloctoSDK.ethereum
  .request({
    method: 'eth_getUserOperationByHash',
    params: [userOpHash],
  })
```
{% endcode %}

{% code title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getUserOperationByHash",
  "params": [
    // UserOperation object
    {
      sender,
      nonce,
      initCode,
      callData,
      callGasLimit,
      verificationGasLimit,
      preVerificationGas,
      maxFeePerGas,
      maxPriorityFeePerGas,
      paymasterAndData,
      signature
    },
    // The EntryPoint address
    entryPoint,
    // The block number this UserOperation was included in
    blockNumber,
    // The block hash this UserOperation was included in
    blockHash,
    // The transaction this UserOperation was included in
    transactionHash,
  ]
}
```
{% endcode %}

## Sample Code

{% embed url="https://codesandbox.io/s/github/blocto/blocto-sdk/tree/main/examples/with-evm-blocto-send-useroperation" %}
