# Flow

{% hint style="warning" %}
Note that Blocto Android SDK for Flow is still in **Beta**.\
APIs are subject to breaking changes.
{% endhint %}

We highly recommend using [FCL-Android](https://github.com/portto/fcl-android) to interact with Flow blockchain instead of using Blocto SDK directly due to Flow blockchain's unique transaction flow for web. Blocto provides both native  (has Blocto wallet app installed) and web experience (Blocto wallet app not installed). When Blocto app is installed, Blocto-SDK-Flow is used as a `Wallet Provider` under the hood within FCL-Android.

{% hint style="warning" %}
FCL-Android and Blocto-SDK-Flow only supports Blocto wallet app version `3.12.0` and above.
{% endhint %}

### What can I do with Blocto Android Flow SDK?

* **Get access to all the blockchain interactions**
  * Interact with Flow networks (Mainnet, Testnet)
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
  Once you've integrated with Blocto Android SDK, your users can manage their assets easily and securely through Blocto App. Your dApp can tap into the vast blockchain ecosystem instantly.
