---
description: Sign and send transaction through Blocto.
---

# Send Transaction

## Sign and Send Transaction

Blocto provides a freemium experience so that they don't have to buy crypto to pay for fees before they try out your dApp \([See how meta-transaction works](../../technical-documents/contract-wallet.md#meta-transaction)\). Hence, your dApps have to sign and send transaction in one-line code so that Blocto is able to pay transaction fee for users.

```typescript
import { SystemProgram, Account, Transaction } from '@solana/web3.js'

const walletPublicKey = window.solana.publicKey

const transaction = new Transaction()
const instruction = SystemProgram.createAccount({
  fromPubkey: walletPublicKey,
  ...
})
transaction.add(instruction)

const txHash = await window.solana.signAndSendTransaction(transaction)
console.log(txHash) // tx hash
```

## Convert to Program Wallet Transaction

Sometimes, you probably need to call `web3.partialSign` to authenticate System Program. Meta-Transaction is going to wrap your instructions into invoke instruction so that it can be sent to program wallet properly. Therefore, your dApps have to call `convertToProgramWalletTransaction` to the correct message before calling `signAndSendTransaction`.

```typescript
transaction.recentBlockhash = (await connection.getRecentBlockhash(commitment)).blockhash
transaction.feePayer = window.solana.publicKey
if (signers.length > 0) {
  transaction = await window.solana.convertToProgramWalletTransaction(transaction)
  transaction.partialSign(...signers)
}

const txHash = await window.solana.signAndSendTransaction(transaction)
console.log(txHash) // tx hash
```

