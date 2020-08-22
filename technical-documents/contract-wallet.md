---
description: Blocto wallets are built with smart contracts
---

# Contract Wallet

Blocto wallets are built with smart contracts. This gives Blocto wallets a lot of advantages over regular private-key wallets.

### Meta Transaction

Transaction fee is troublesome and confusing for blockchain beginners.

Users nowadays are familiar with freemium model, where users try out apps for free and pay for value-added features if they like the app enough.

Due to the decentralized nature of blockchain and dApps, users have to pay for the transaction fees when they interact with dApps. The amount might not be much, but users still have to purchase the crypto needed somewhere, so they can pay for the fees.

Austin Griffith introduced [meta-transaction](https://medium.com/@austin_48503/ethereum-meta-transactions-90ccf0859e84) back in 2018. Meta-transaction makes fee subsidization possible. Here's how it works:

1. The wallet itself is a smart contract
2. The owner holds a key to control the wallet contract
3. When the owner sends a transaction, he signs the message with his key but instead of sending the signed message to blockchain directly, he sends it to a 3rd party relayer who pays for the transaction fee and actually send the transaction onto blockchain.
4. The signed message is verified by the wallet contract to make sure that it actually came from the rightful owner.

Blocto makes the fee payment flexible by incorporating meta-transaction. In Blocto, the transaction fee can be payed by either:

* dApp builder, to improve user experience and the fee is treated like dApp operation cost
* Blocto, as our user acquisition cost
* User, which is similar to normal transactions. However, they can pay with currencies other than the blockchain's native currency. They can also top-up their balance with Apple/Google in-app purchase.

### Ownership

The owner of contract wallets can be designed flexibly, to provide additional security or convenience.

In Blocto, contract wallet makes it possible for us to build our [mixed-custodial model for key management](key-management.md#mixed-custodial). With mixed-custodial key management model, we get the advantages of both custodial and non-custodial wallets.

### Batch Transaction

With contract wallet, Blocto can combine multiple transactions into a single transaction easily, for the following advantages:

1. Save gas fee
2. Make multiple transactions atomic, so they either all succeed or all fail

