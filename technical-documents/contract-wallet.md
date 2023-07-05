---
description: Blocto wallets are built with chain-agnostic account abstraction
---

# Account Abstraction (EVM)

Blocto wallets are built with chain-agnostic account abstraction. On chains with native account abstraction, such as [Aptos](https://aptos.dev) and [Flow](https://flow.com/), Blocto utilizes the native account system to provide account abstraction functionalities; On chains without native account abstraction, such as Ethereum, Polygon and Solana, Blocto builds uses smart contracts to achieve account abstraction. This gives Blocto wallets a lot of advantages over regular private-key wallets.

This article describes our [ERC-4337](https://eips.ethereum.org/EIPS/eip-4337) compliant smart contract wallet on EVM.

### Meta Transaction

Transaction fee is troublesome and confusing for blockchain beginners.

Users nowadays are familiar with freemium model, where users try out apps for free and pay for value-added features if they like the app enough.

Due to the decentralized nature of blockchain and dApps, users have to pay for the transaction fees when they interact with dApps. The amount might not be much, but users still have to purchase the crypto needed somewhere, so they can pay for the fees.

Austin Griffith introduced [meta-transaction](https://medium.com/@austin\_48503/ethereum-meta-transactions-90ccf0859e84) back in 2018. Meta-transaction makes fee subsidization possible. Here's how it works:

1. The wallet itself is a smart contract
2. The owner holds a key to control the wallet contract
3. When the owner sends a transaction, he signs the message with his key but instead of sending the signed message to blockchain directly, he sends it to a 3rd party relayer who pays for the transaction fee and actually sends the transaction onto blockchain.
4. The signed message is verified by the wallet contract to make sure that it actually came from the rightful owner.

Blocto makes the fee payment flexible by incorporating meta-transaction. In Blocto, the transaction fee can be paid by either:

* dApp builder, to improve user experience and the fee is treated like dApp operation cost
* Blocto, as our user acquisition cost
* User, which is similar to normal transactions. However, they can pay with currencies other than the blockchain's native currency. They can also top-up their balance with Apple/Google in-app purchase.

### Ownership

The owner of contract wallets can be designed flexibly, to provide additional security or convenience.

In Blocto, contract wallet makes it possible for us to build our [mixed-custodial model for key management](key-management.md#mixed-custodial). With mixed-custodial key management model, we get the advantages of both custodial and non-custodial wallets.

In any of the supported chains (Ethereum, Flow, Solana and Tron), there are three different roles on the contract wallets:

1. **Device Key or Signer Key**\
   The key stored on users device. In custodial mode, this key is also managed by Blocto backend service and gets distributed to users' local device when users logs in. In non-custodial mode, each device has its own device private key and is stored only on the device keychain or keystore.
2. **Co-signer Key**\
   \*\*\*\*The key managed by Blocto backend service. On EVM chains, this key is also used as the fee payer account for meta-transactions.
3. **Recovery Key**\
   \*\*\*\*The key used to reset account access. In custodial mode, this key is managed by Blocto backend service and is used when user sets up non-custodial mode or reset non-custodial mode. In non-custodial mode, the recovery key is generated from users' device and is encrypted with the recovery password set by the users. The encrypted recovery key is then sent to users' email for backup.

For different account operations, different keys are used:

1. **Send Transaction**\
   \*\*\*\*To send out transactions from users' account, the transaction has to be signed with both users' **device key** and **co-signer** key. The device key is kept on users' devices, while the co-signer key is kept in Blocto backend service and is authorized with users' login access token.\\
2. **Setup or reset non-custodial mode**\
   \*\*\*\*When a user sets up or resets non-custodial mode, a new pair of device key and recovery key is generated on the user's device. The new keys get registered into the contract wallet as the new owner keys with the authorization of the old recovery key. Once the setup is complete, the old recovery key and old device keys will not work anymore.

### Batch Transaction

With contract wallet, Blocto can combine multiple transactions into a single transaction easily, for the following advantages:

1. Save gas fee
2. Make multiple transactions atomic, so they either all succeed or all fail

### Acknowledgment

Blocto's EVM contract wallet was modified from [Dapper Wallet](https://github.com/dapperlabs/dapper-contracts).
