# useWallet

* required provider: WalletProvider
* parameters
  * wallets: AdapterInjection\[] (required)
  * enables: string\[] (required)
  * autoConnect: boolean
  * localStorageKey: string (default: 'walletId')
  * onError: (error: WalletError, adapter?: Adapter) => void

### Properties

| Name               | Description                                         | Type                         | Default |
| ------------------ | --------------------------------------------------- | ---------------------------- | ------- |
| autoConnect        | Connect wallet automatically when refresh dapp site | boolean;                     | false   |
| wallets            | Wallet list to be used                              | Wallet\[];                   | \[]     |
| enables            | Blockchain list to be used                          | string\[];                   | \[]     |
| wallet             | Selected wallet                                     | Wallet \| null;              | null    |
| address            | Address                                             | string \| null;              | null    |
| connected          | State that wallet is connected                      | boolean;                     | false   |
| connecting         | State that wallet is connecting                     | boolean;                     | false   |
| disconnecting      | State that wallet is disconnecting                  | boolean;                     | false   |
| selectedBlockChain | Connected blockchain                                | string \| null;              | null    |
| isNotInstalled     | State that wallet is not installed                  | boolean                      | false   |
| provider           | Connected provider                                  | unknown                      | null    |
| selectedChainId    | Connected network                                   | string\[] \| string \| null; | null    |

### Methods

<table><thead><tr><th width="204">Name</th><th width="216">Description</th><th width="329">Type</th></tr></thead><tbody><tr><td>selectWallet</td><td>Choose wallet type to connect</td><td><p>(</p><p>    walletId: string | null,</p><p>    chainId: string | null,</p><p>    callback?: (</p><p>      error: Error,</p><p>      typeError: TypeConnectError,</p><p>      addNetwork?: () => Promise&#x3C;void>,</p><p>      provider?: unknown,</p><p>    ) => Promise&#x3C;void>,</p><p>  ) => void;</p></td></tr><tr><td>getAvailableWallet</td><td>Retrieve list of available wallets to choose from<br><br>Note: this method is only available in Coin98 Wallet</td><td>() => Promise&#x3C;WalletReturnType&#x3C;AvailableWalletType, string>>;</td></tr><tr><td>disconnect</td><td>Disconnect wallet</td><td>() => Promise;</td></tr><tr><td>connect</td><td>Connect to the selected wallet</td><td>(callbackAddChain?: (error: Error) => Promise) => Promise;</td></tr><tr><td>switchNetwork</td><td>Request wallet to switch its current chain to the requested chain (when switching between EVM chains)</td><td><p>(</p><p>    chainId: string,</p><p>    callBackAddChain?: (</p><p>      error: Error,</p><p>      typeError?: TypeConnectError,</p><p>      addNetwork?: () => Promise&#x3C;void>,</p><p>      provider?: unknown,</p><p>    ) => Promise&#x3C;void>,</p><p>  ) => Promise&#x3C;WalletReturnType&#x3C;boolean, string> | undefined>;</p></td></tr><tr><td>switchBlockchain</td><td>Request wallet to switch its current chain to the requested chain (when switching between EVM chain and non-EVM chain)<br><br>Note: this method is only available in Coin98 Wallet</td><td>(blockChainName: string, callback?: (error: Error) => Promise) => Promise;</td></tr><tr><td>requestInstall</td><td>Show the install wallet request modal</td><td>(value: boolean) => void</td></tr><tr><td>sendTransaction</td><td>Creates a new transaction confirmation from the user's wallet address</td><td>(transaction: Transaction)=> Promise&#x3C;WalletReturnType&#x3C;string[] | string, string>>;</td></tr><tr><td>signMessage</td><td>Presents a plain text signature challenge to the user and returns the signed response</td><td>(message: string) => Promise&#x3C;WalletReturnType&#x3C;string[], string>>;</td></tr><tr><td>signTypedData</td><td>Presents a structured and readable data message for the user to sign, then returns a signed response</td><td>(msgParams: TypedMessageV3 | TypedMessageV4 | TypedMessage[], type: 'v1' | 'v3' | 'v4' ) => Promise&#x3C;WalletReturnType&#x3C;string, string>>;</td></tr><tr><td>ethSign</td><td>Presents a data message for the user to sign arbitrary messages</td><td>(message: string) => Promise&#x3C;WalletReturnType&#x3C;string, string>>;</td></tr><tr><td>watchAsset</td><td>Request to track a specific token in wallet</td><td>(params: WatchAssetType)=>Promise&#x3C;WalletReturnType&#x3C;boolean, string>>;</td></tr><tr><td>getEncryptionPublicKey</td><td>Requests to get user wallet address's public encryption key</td><td>() => Promise&#x3C;WalletReturnType&#x3C;string, string>>;</td></tr><tr><td>ethDecrypt</td><td>Request to decrypt a specific encrypted message</td><td>(message: string, address?: string) => Promise&#x3C;WalletReturnType&#x3C;unknown, string>>;</td></tr></tbody></table>

