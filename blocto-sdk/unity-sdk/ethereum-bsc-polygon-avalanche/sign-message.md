# Sign Message

{% hint style="info" %}
Make sure you [initialize Blocto SDK](getting-started.md) first
{% endhint %}

Once your app is connected to Blocto wallet, it can sign message on behalf of the user, with the user's permission.

To sign a message, the app must specify the sign type. All available types are listed below.

| Sign Type                                                                | Type                        | Message                  |
| ------------------------------------------------------------------------ | --------------------------- | ------------------------ |
| ETH Sign                                                                 | `EvmSignType.ETH_SIGN`      | hex string (`0x` prefix) |
| Personal Sign                                                            | `EvmSignType.PERSONAL_SIGN` | plain string             |
| Sign Typed Data v3                                                       | `EvmSignType.TYPED_DATA_V3` | json string              |
| Sign Typed Data v4                                                       | `EvmSignType.TYPED_DATA_V4` | json string              |
| <p>Sign Typed Data</p><p>(currently identical to Sign Typed Data v4)</p> | `EvmSignType.TYPED_DATA`    | json string              |

```csharp
bloctoWalletProvider.SignMessage(
    message: message,
    signTypeEnum: [SignTypeEnum.Eth_Sign/SignTypeEnum.Personal_Sign/SignTypeEnum.SignTypeData/...],
    address: walletAddress,
    callback: signature => {
        Debug.Log($"Signature: {signature}");
    });
```

For dApps relying on `signMessage` for off-chain authentication, Blocto follows [EIP-1654](https://github.com/ethereum/EIPs/issues/1654). To verify the signature, you need to call a method on the wallet contract to check if the signature came from a rightful owner of the wallet contract.
