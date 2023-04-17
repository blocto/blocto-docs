# Getting Started

In this guide we show how to use it.

A sample application is available at: [https://github.com/portto/blocto-unity-sdk](https://github.com/portto/blocto-unity-sdk)

### Requirements <a href="#requirements-a-hrefrequirements-idrequirementsa" id="requirements-a-hrefrequirements-idrequirementsa"></a>

* .Net Core version >= 2.1
* iOS version >= 13
* Android version >= 7.1

### Release Page <a href="#release-page" id="release-page"></a>

Coming soon

### Import .unitypackage <a href="#import-unitypackage" id="import-unitypackage"></a>

You can import **Custom Packages**, which are made by portto using Unity. More description at [unity document](https://docs.unity3d.com/Manual/AssetPackagesImport.html).

Choose **Assets > Import Package >** to import both types of package.

### Wallet Provider

1. [Register app id](https://docs.blocto.app/blocto-sdk/register-app-id) (bloctoSDKAppId) in order to init `BloctoWalletProvider`
2. Create `BloctoWalletProvider` instance.

```csharp
using Blocto.Sdk.Core.Extension;
using Blocto.Sdk.Core.Utility;
using Blocto.Sdk.Aptos;
        
_bloctoWalletProvider = BloctoWalletProvider.CreateBloctoWalletProvider(
            gameObject: gameObject,
            env: EnvEnum.Mainnet,
            bloctoAppIdentifier:Guid.Parse("your appId")
        ); 
```

Both EnvEnum.Testnet and EnvEnum.Devnet use Aptos testnet.
