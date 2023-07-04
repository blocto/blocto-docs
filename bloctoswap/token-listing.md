---
description: List your tokens in BloctoSwap
---

# Token Listing

#### Add your token to BloctoSwap

To begin, it is necessary for you to provide us with your Fungible tokens contract. Fungible tokens on the Flow blockchain is implemented using the [**FungibleToken**](https://developers.flow.com/flow/core-contracts/fungible-token) interface. For examples of contracts, please take a look at the [**Flow USD (FUSD) repository**](https://github.com/onflow/fusd). This repository contains the Cadence source code for the FUSD contract and accompanying transactions.

Once your contract has been deployed, you can proceed to add a new swap pair to BloctoSwap. The process for doing so is as follows:

1.  Go to [**BloctoSwap Listings**](https://github.com/blocto/bloctoswap-listings) repository. This is where we maintain the official list of tokens that are supported by BloctoSwap. Add a new entry to the `data/{network}/tokens.json` file. This file contains a list of tokens that BloctoSwap supports.

    The token entry should be in the following format:

    ```jsx
    {
      // name of the token that is used in the contract
      "name": "TestToken",
      // an user friendly version of token name.
      "displayName": "Test Token",
      // symbol to be displayed
      "symbol": "TEST",
    	// the address that your token contract deployed to
      "address": "0x1234567890abcde",
      "decimals": 8,
      // vault storage path
      "vaultPath": "/storage/testTokenVault",
      // receiver storage path
      "receiverPath": "/public/testTokenReceiver",
      // balance storage path
      "balancePath": "/public/testTokenBalance",
      // we use it internally, please always set it to true
      "shouldCheckVaultExist": true 
    },
    ```
2. After you done, send a GitHub pull request to our [**BloctoSwap Listings**](https://github.com/blocto/bloctoswap-listings) repository. Your pull request will be reviewed by our team, and if everything looks good, your new token will be added to the list.

#### Add new swap pair to BloctoSwap

To add a new swap pair to BloctoSwap, follow these steps:

1.  Go to [**BloctoSwap Listings**](https://github.com/blocto/bloctoswap-listings) repository, and add a new entry in `data/{network}/pairs.json`. This file contains a list of all the swap pairs that BloctoSwap supports. The format of the entries in `pairs.json` is simple: each entry represents the address that contract deploy to, the swap contract name, and includes information such as name of tokens that can be swapped and the pairs' contract addresses. You should follow this format when adding your new swap pair to the list.

    A swap pair entry should follow this format:

    ```jsx
    {
      // name of the swap pair used in the contract
      "name": "FlowTestSwapPair",
      "token0": "FlowToken",
      "token1": "TestToken",
    	// the address that your token contract deployed to
      "address": "0x1234567890abcde",
      // optional liquidity token field. Use "null" for default.
      "liquidityToken": null | "LiqudityToken"
    },
    ```

    For contract examples, refer to the [Starly contract repository](https://github.com/StarlyIO/starly-contracts/blob/master/contracts/FusdUsdtSwapPair.cdc). This repository contains example contract code that you can use as a reference when creating your own swap pair contract. You can also use this code as a starting point for your own contract, if you prefer.
2. After you done, send a GitHub pull request to our [**BloctoSwap Listings**](https://github.com/blocto/bloctoswap-listings) repository. This is where we maintain the official list of all swap pairs that are supported by BloctoSwap. Your pull request will be reviewed by our team, and if everything looks good, your new swap pair will be added to the list.
