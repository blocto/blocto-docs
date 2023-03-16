---
description: Find answers here to general dev questions (CONSTANTLY UPDATING)
---

# FAQ

We have gathered commonly asked questions regarding Blocto SDK integration in this section. If you do not find answers here, you may post questions in our Blocto Discord [#dev-discussion](https://discord.com/channels/720454370650619984/1002112367502565466) Channel or reach out for assistance in your project's dedicated assistance channel.



## **General**

1.  Q: How long does the Blocto SDK integration take in general?

    &#x20;

    Answer:&#x20;

    The time needed for integrating Blocto-SDK may vary, usually: a couple hours for experienced engineers and 2-3 days for inexperienced engineers.


2.  Q: Does Blocto have an API that allows 3rd parties to integrate Blocto Wallet or transactions?



    Answer:&#x20;

    No.\

3.  Q: Can I integrate Blocto with the Angular framework?&#x20;



    Answer:

    Yes.\

4.  Q: Can I set my Blocto testnet account as non-custodial?&#x20;



    Answer:

    Yes, you can download Android/iOS apps for dev environment.\
    Android: [https://docs.blocto.app/blocto-environment#android-app](https://docs.blocto.app/blocto-environment#android-app)\
    iOS: [https://docs.blocto.app/blocto-environment#ios-app](https://docs.blocto.app/blocto-environment#ios-app)\

5.  Q: I am testing on testnet, where and how do I get Blocto Points for payments?&#x20;



    Answer:&#x20;

    You will receive 1,000 Blocto Points upon registration. If additional points are required, you may follow the steps below:\


    _1/ Logs in on the Developer dashboard._

    _2/ Click the "Blocto points faucet" tab._

    _3/ Input a staging email (user AAA) and click the submit button._

    _4/ View the response and AAA gets Blocto points._\
    __
6.  Q: Is Blocto Wallet an Injected Wallet?



    Answer:

    Yes, for Blocto Android/iOS App, there is an in-app browser.\

7.  Q: Length error regarding signing and verifying the message.\


    Context:&#x20;

    _`I followed the instructions at: https://docs.blocto.app/blocto-sdk/javascript-sdk/evm-sdk/sign-message`_

    _`However, when verifying the message, there is an error about the length of the signature. Specifically:`_

    _`The signature after signing with Blocto's provider is 262 characters long (including 0x).`_

    _`The signature in the example (and accepted in the library) is 132 characters long (including 0x).`_



    Answer:&#x20;

    _1/ the library combines two verifications:_

    _Step 1. EOA signature_

    _Step 2. EIP1271 signature_\
    __

    _2/ The Blocto signature must fail in step 1. But it will work in step 2. The number of Blocto signature is 2, which is equal to 65\*2 bytes = 260 chars (without 0x)._

    __

    _3/ the lib (https://github.com/dapperlabs/dappauth.js/blob/master/index.js#L36) which did Step 2, can verify the signature from the private key wallet and the smart contract wallet at the same time. So you can ignore it if you only use Blocto._



    Reference:

    https://docs.blocto.app/blocto-sdk/javascript-sdk/evm-sdk/login-register\

8.  Q: Failed to recover the account that signs the data.



    Answer:

    Smart contract wallet doesn't have private key. Instead, please follow ERC1654 to verify the signature ([github](https://github.com/dapperlabs/dappauth.js)).\


## Unity SDK

1.  Q: (Android) Why is your Blocto app not returning back to my DemoApp?&#x20;



    Answer:

    Kindly help to follow the below steps and you should be able to be redirected from Blocto to your app:&#x20;

    1.  Please use the latest code from GitHub repo and download the latest version of Blocto dev app from Google Play. (screenshot as below)\


        <figure><img src="https://lh3.googleusercontent.com/EtxnG0Ppcl84KOeDDYUPxMmsLlGnQ9_jbzGs0K60djG6DNExoHLnrkh6AQiv2dDbIhwzub6NBeXFKEMDLR-o5xjTr1d93CfTv9oNdPbt32BzHpkE9cpK8dodhYkDQtDJ8yl5NUkqqOs57DLyk6BcyLw" alt=""><figcaption></figcaption></figure>
    2.  Then make sure the description of App - Android on Blocto dashboard matches with your unity project’s Identification on unity editor (check screenshot below).\


        <figure><img src="https://lh4.googleusercontent.com/7DIg3uSIbCYFXoFlLY5ygIaqnepBF3TZ7rm5N_lOqz2cKs2Gw-i5ESlEVJJ_miZcGVfxqxjpDzdLC0fbWeEgOe_2G0wT8LFxJwJnbRu_Tir_fg1psJV_5mDJcOBgX9zEK_jJhK4mCWAYSKl6Ig27hWw" alt=""><figcaption></figcaption></figure>

## Chain-Related Specifics

* **FLOW**
  1. Q: Is there an official documentation for me to follow when using the Flow FCL to connect with Blocto?\
     \
     Answer:\
     Please see here: [https://docs.blocto.app/](https://docs.blocto.app/)\
     \

* **APTOS**
  1.  Q: How do I update MainNet App ID to BloctoSDK? ([doc](https://docs.blocto.app/blocto-sdk/javascript-sdk/aptos/provider))



      Answer:

      You need to change your chainID from 2 (Testnet) to 1(Mainnet).

      👉More [updates](https://github.com/aptos-labs/aptos-wallet-adapter)

      \


\


\


\


\


\


\




\


\


