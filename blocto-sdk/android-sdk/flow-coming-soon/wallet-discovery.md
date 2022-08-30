# Wallet Discovery

When you initialize FCL, it's required to provide a list of wallet providers. The following is an example of adding `Blocto` as a wallet provider.

```kotlin
val walletProviderList = listOf(Blocto.getInstance(YOUR_BLOCTO_APP_ID))
```

{% hint style="info" %}
To obtain a **Blocto App ID**, please refer to the [instructions](../../register-app-id.md).
{% endhint %}

