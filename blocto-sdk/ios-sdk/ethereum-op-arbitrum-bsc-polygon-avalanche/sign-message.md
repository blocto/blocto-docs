# Sign Message

{% hint style="warning" %}
Make sure you [initialize Blocto SDK](broken-reference) first
{% endhint %}

Once your app is connected to Blocto wallet, it can send sign EVMBase message on behalf of the user, with the user's permission.

Currently we support these sign types:

```swift
public enum EVMBaseSignType: String {
    case sign // stand for eth_sign
    case personalSign = "personal_sign"
    case typedSignV3 = "typed_data_sign_v3"
    case typedSignV4 = "typed_data_sign_v4"
    case typedSign = "typed_data_sign" // same as the latest, it's same as default v4 for now.
}
```

{% hint style="warning" %}
When using **`sign`** type, please make sure input message is hex string with \*\*`0x`\*\*prefix.
{% endhint %}

{% hint style="warning" %}
We highly recommend you specify version when using typedSign. Because we can't make sure what version of Blocto Wallet app the user installed. When using \*\*`typedSign`\*\*we just use the latest version of typedSign, but the "latest" typedSign in user's current Blocto Wallet app might not match with the "latest" typedSign in BloctoSDK. In such case, it may lead to unexpected result.
{% endhint %}

Let's take Ethereum for example. Arbitrum, Optimism, BSC, Polygon and Avalanche use the same **`signMessage`** method as below.

### Sign message

#### Sign

```swift
BloctoSDK.shared.evm.signMessage(
    blockchain: .ethereum,
    from: "0x...", // user address
    message: "0x50...3A", // must be hex string with 0x prefix
    signType: .sign) { [weak self] result in
        guard let self = self else { return }
        switch result {
        case let .success(signature):
            // handle signature here.
        case let .failure(error):
            // handle error here.
        }
    }
```

#### Personal sign

```swift
BloctoSDK.shared.evm.signMessage(
    blockchain: .ethereum,
    from: "0x...", // user address
    message: "any message",
    signType: .personalSign) { [weak self] result in
        guard let self = self else { return }
        switch result {
        case let .success(signature):
            // handle signature here.
        case let .failure(error):
            // handle error here.
        }
    }
```

#### Typed sign v3

```swift
BloctoSDK.shared.evm.signMessage(
    blockchain: .ethereum,
    from: "0x...", // user address
    message: #"{ "abc": 123 }"#, // must be valid json format.
    signType: .typedSignV3) { [weak self] result in
        guard let self = self else { return }
        switch result {
        case let .success(signature):
            // handle signature here.
        case let .failure(error):
            // handle error here.
        }
    }
```

#### Typed sign v4

```swift
BloctoSDK.shared.evm.signMessage(
    blockchain: .ethereum,
    from: "0x...", // user address
    message: #"{ "abc": 123 }"#, // must be valid json format.
    signType: .typedSignV4) { [weak self] result in
        guard let self = self else { return }
        switch result {
        case let .success(signature):
            // handle signature here.
        case let .failure(error):
            // handle error here.
        }
    }
```
