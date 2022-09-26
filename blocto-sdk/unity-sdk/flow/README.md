# Flow

{% hint style="warning" %}
Note that Blocto unity SDK for Flow is still in **Beta**.\
APIs are subject to breaking changes.
{% endhint %}

We highly recommend using [FCL-Unity](https://github.com/portto/blocto-unity-sdk/tree/main/Assets/Plugins/Flow) to interact with Flow blockchain instead of using Blocto Flow SDK directly due to Flow blockchain's unique transaction flow for web. Blocto serves both native experience (has Blocto wallet app installed) and web experience (not install Blocto wallet app), so we handled that troublesome in [BloctoWalletProvider](https://github.com/portto/blocto-unity-sdk/tree/main/Assets/Plugins/Blocto.Sdk/Flow) for you. Blocto Flow SDK will be used inside [BloctoWalletProvider](https://github.com/portto/blocto-unity-sdk/tree/main/Assets/Plugins/Blocto.Sdk/Flow) from [FCL-Unity](https://github.com/portto/blocto-unity-sdk/tree/main/Assets/Plugins/Flow).

#### What can I do with Blocto unity flow SDK?

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
  Once you've integrated with Blocto Unity SDK, your users can manage their assets easily and securely through Blocto App. Your dApp can tap into the vast blockchain ecosystem instantly.
