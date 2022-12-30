# Getting Started

## Getting Started

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

\## Wallet Provider

1. [Register app id](https://docs.blocto.app/blocto-sdk/register-app-id) (bloctoSDKAppId) in order to init `BloctoWalletProvider`
2. Create `BloctoWalletProvider` instance.

```csharp
using Blocto.Sdk.Core.Extension;
using Blocto.Sdk.Core.Utility;
using Blocto.Sdk.Evm;
        
var bloctoWalletProvider = BloctoWalletProvider.CreateBloctoWalletProvider(
                                                    gameObject: gameObject,
                                                    env: EnvEnum.Devnet,
                                                    bloctoAppIdentifier:Guid.Parse("{your app id}")
                                                );
bloctoWalletProvider.Chain = ChainEnum.Ethereum;
```

regarding the blockchain supported by Evm SDK, we provide ChainEnum.Ethereum (Ethereum), ChainEnum.BSC (Bsc), ChainEnum.Polygon (Polygon), ChainEnum.Avalanche (Avalanche).

in Ethereum`dev` for Rinkeby and `prod` for mainnet.\
in BSC`dev` for testnet and `prod` for mainnet.\
in Polygon`dev` for Mumbai and `prod` for mainnet.\
in Avalanche`dev` for FUJI Testnet and `prod` for mainnet.
