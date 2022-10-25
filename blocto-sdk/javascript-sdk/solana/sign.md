# Sign

Sign have `partialSign` and `signatures`
Both sign and partialSign are used to add signatures to a transaction.

A transaction needs someone to pay the fee. In case the feePayer is not provided for a transaction then the first signer will be used for paying the fee.

partialSign is used to add signatures to a transaction. For instance if you are using createAccount then you need to make sure the new account is also a signer.

Install from npm/yarn

```bash
$ yarn add @blocto/sdk @solana/web3.js
```

```javascript
import BloctoSDK from "@blocto/sdk";
import {
  Transaction,
  PublicKey,
  TransactionInstruction,
  Keypair,
} from "@solana/web3.js";

const bloctoSDK = new BloctoSDK({
  solana: {
    net: "devnet",
  },
});

const signHandler = async () => {
  try {
    const {
      value: { blockhash },
    } = await bloctoSDK?.solana?.request({
      method: "getRecentBlockhash",
    });

    const transaction = new Transaction();
    const publicKey = new PublicKey(address);

    const newKeypair = new Keypair();
    const newAccountKey = newKeypair.publicKey;

    const memoInstruction = new TransactionInstruction({
      keys: [
        { pubkey: publicKey, isSigner: false, isWritable: true },
        { pubkey: newAccountKey, isSigner: true, isWritable: true },
      ],
      data: Buffer.from("Data to send in transaction", "utf-8"),
      programId: new PublicKey("MemoSq4gqABAXKb96qnH8TysNcWxMyWCqXgDLGmfcHr"),
    });
    transaction.add(memoInstruction);

    const createdPublicKey = newAccountKey.toBase58();

    transaction.feePayer = publicKey;
    transaction.recentBlockhash = blockhash;

    const converted =
      await bloctoSDK?.solana?.convertToProgramWalletTransaction(transaction);

    if (converted) {
      converted?.partialSign(newKeypair);
      const transactionId = await bloctoSDK?.solana?.signAndSendTransaction(
        converted
      );
    }
  } catch (error) {
    console.error(error);
  }
};

const PartialSignAndWrap = async () => {
  try {
    const {
      value: { blockhash },
    } = await bloctoSDK?.solana?.request({
      method: "getRecentBlockhash",
    });

    const transaction = new Transaction();
    const publicKey = new PublicKey(address);

    const newKeypair = new Keypair();
    const newAccountKey = newKeypair.publicKey;

    const memoInstruction = new TransactionInstruction({
      keys: [
        { pubkey: publicKey, isSigner: false, isWritable: true },
        { pubkey: newAccountKey, isSigner: true, isWritable: true },
      ],
      data: Buffer.from("Data to send in transaction", "utf-8"),
      programId: new PublicKey("MemoSq4gqABAXKb96qnH8TysNcWxMyWCqXgDLGmfcHr"),
    });
    // at least 2 instructions to make them wrapped by backend
    transaction.add(memoInstruction);
    transaction.add(memoInstruction);

    const createdPublicKey = newAccountKey.toBase58();

    transaction.feePayer = publicKey;
    transaction.recentBlockhash = blockhash;

    const converted =
      await bloctoSDK?.solana?.convertToProgramWalletTransaction(transaction);

    if (converted) {
      converted?.partialSign(newKeypair);
      const transactionId = await bloctoSDK?.solana?.signAndSendTransaction(
        converted
      );
    }
  } catch (error) {
    console.error(error);
  }
};
```

{% embed url="https://codesandbox.io/s/sol-sign-p02rlb?file=/src/App.js" %}
