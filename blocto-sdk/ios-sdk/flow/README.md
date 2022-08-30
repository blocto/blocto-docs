# Flow

{% hint style="warning" %}
Note that Blocto iOS SDK for Flow is still in **Beta**.\
APIs are subject to breaking changes.
{% endhint %}

We highly recommend using [FCL-Swift](https://github.com/portto/fcl-swift) to interact with Flow blockchain instead of using Blocto Flow SDK directly due to Flow blockchain's unique transaction flow for web. Blocto serves both native experience (has Blocto wallet app installed) and web experience (not install Blocto wallet app), so we handled that troublesome in [BloctoWalletProvider](https://github.com/portto/fcl-swift/blob/main/Sources/FCL-SDK/WalletProvider/BloctoWalletProvider.swift) for you. Blocto Flow SDK will be used inside [BloctoWalletProvider](https://github.com/portto/fcl-swift/blob/main/Sources/FCL-SDK/WalletProvider/BloctoWalletProvider.swift) from [FCL-Swift](https://github.com/portto/fcl-swift).

### What can I do with Blocto iOS Flow SDK?

* **Get access to all the blockchain interactions**
  * Interact with Flow networks (Mainnet-beta, Testnet)
  * Sign messages
  * Send transactions
  * Query smart contract state
  * Mutate smart contract state
  * ... and a lot more
* **Seamless onboarding experience**\
  Users can sign up easily with email and start exploring you dApp in seconds.
* **Fee subsidization**\
  You have the option to pay transaction fee for your users and provide a better experience. In that case, we will generate daily fee reports for you to review.
* **Integrated payment**\
  Get paid easily with our payment APIs. Users can pay easily with credit cards.
* **Connected to Blocto App**\
  Once you've integrated with Blocto iOS SDK, your users can manage their assets easily and securely through Blocto App. Your dApp can tap into the vast blockchain ecosystem instantly.
