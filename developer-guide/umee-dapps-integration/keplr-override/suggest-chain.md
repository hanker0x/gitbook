# Suggest Chain

_Warning: This is an experimental feature._

Coin98's 'suggest chain' feature allows front-ends to request adding new Cosmos-SDK based blockchains that isn't natively integrated to Coin98 extension.\
If the same chain is already added to Coin98, nothing will happen. If the user rejects the request, an error will be thrown.

This allows all Cosmos-SDK blockchains to have permissionless, instant wallet and transaction signing support for front-ends.

```
interface ChainInfo {
    readonly rpc: string;
    readonly rest: string;
    readonly chainId: string;
    readonly chainName: string;
    /**
    * This indicates the type of coin that can be used for stake.
    * You can get actual currency information from Currencies.
    */
    readonly stakeCurrency: Currency;
    readonly walletUrlForStaking?: string;
    readonly bip44: {
        coinType: number;
    };
    readonly alternativeBIP44s?: BIP44[];
    readonly bech32Config: Bech32Config;
    
    readonly currencies: AppCurrency[];
    /**
    * This indicates which coin or token can be used for fee to send transaction.
    * You can get actual currency information from Currencies.
    */
    readonly feeCurrencies: Currency[];
    
    /**
    * This is used to set the fee of the transaction.
    * If this field is empty, it just use the default gas price step (low: 0.01, average: 0.025, high: 0.04).
    * And, set field's type as primitive number because it is hard to restore the prototype after deserialzing if field's type is `Dec`.
    */
    readonly gasPriceStep?: {
        low: number;
        average: number;
        high: number;
    };
    
    /**
    * Indicate the features supported by this chain. Ex) cosmwasm, secretwasm ...
    */
    readonly features?: string[];
}
```

```
experimentalSuggestChain(chainInfo: SuggestingChainInfo): Promise<void>
```

**Usage examples and recommendations**

<table><thead><tr><th width="150">Key</th><th width="273">Example Value</th><th>Note</th></tr></thead><tbody><tr><td>rpc</td><td>http://123.456.789.012:26657</td><td>Address of RPC endpoint of the chain. Default port is 26657</td></tr><tr><td>rest</td><td>http://123.456.789.012:1317</td><td>Address of REST/API endpoint of the chain. Default port is 1317. Must be enabled in <code>app.toml</code></td></tr><tr><td>chainId</td><td>mychain-1</td><td>Coin98 has a feature which automatically detects when the chain-id has changed, and automatically update to support new chain. However, it should be noted that this functionality will only work when the chain-id follows the {identifier}-{version}(ex.cosmoshub-4) format. Therefore, it is recommended that the chain follows the chain-id format.</td></tr><tr><td>stakeCurrency</td><td>{ coinDenom: "ATOM", coinMinimalDenom: "uatom", coinDecimals: 6, coinGeckoId: "cosmos", }</td><td>Information on the staking token of the chain</td></tr><tr><td>walletUrlForStaking</td><td>https://wallet.keplr.app/#/cosmoshub/stake</td><td>The URL for the staking interface frontend for the chain. If you don't have a staking interface built, you can use <a href="https://github.com/luniehq/lunie-light">Lunie Light (opens new window)</a>which supports Coin98.</td></tr><tr><td>bip44.coinType</td><td>118</td><td>BIP44 coin type for address derivation. We recommend using <code>118</code>(Cosmos Hub) as this would provide good Ledger hardware wallet compatibility by utilizing the Cosmos Ledger app.</td></tr><tr><td>bech32Config</td><td>{ bech32PrefixAccAddr: "cosmos", bech32PrefixAccPub: "cosmos" + "pub", bech32PrefixValAddr: "cosmos" + "valoper", bech32PrefixValPub: "cosmos" + "valoperpub", bech32PrefixConsAddr: "cosmos" + "valcons", bech32PrefixConsPub: "cosmos" + "valconspub"}</td><td>Bech32 config using the address prefix of the chain</td></tr><tr><td>currencies</td><td>[ { coinDenom: "ATOM", coinMinimalDenom: "uatom", coinDecimals: 6, coinGeckoId: "cosmos", }, ]</td><td>(TBD)</td></tr><tr><td>feeCurrencies</td><td>[ { coinDenom: "ATOM", coinMinimalDenom: "uatom", coinDecimals: 6, coinGeckoId: "cosmos", }, ]</td><td>List of fee tokens accepted by the chain's validator.</td></tr><tr><td>gasPriceStep</td><td>{ low: 0.01, average: 0.025, high: 0.03, }</td><td>Three <code>gasPrice</code> values (low, average, high) to estimate transaction fee.</td></tr><tr><td>features</td><td>[]</td><td><code>secretwasm</code> - Secret Network WASM smart contract transaction support <code>ibc-transfer</code> - For IBC transfers (ICS 20) enabled chains. For Stargate (cosmos-sdk v0.40+) chains, Coin98 will check the on-chain params and automatically enable IBC transfers if it’s available) <code>cosmwasm</code> - For CosmWasm smart contract support (currently broken, in the process of being fixed) <code>ibc-go</code> - For chains that use the ibc-go module separated from the cosmos-sdk</td></tr><tr><td>logo</td><td>string</td><td>To display your logo in our extension</td></tr><tr><td>explorer</td><td>string</td><td>To view explorer</td></tr></tbody></table>

Copy and paste example:

```
await window.keplr.experimentalSuggestChain({
    chainId: "mychain-1",
    chainName: "my new chain",
    rpc: "http://123.456.789.012:26657",
    rest: "http://123.456.789.012:1317",
    bip44: {
        coinType: 118,
    },
    bech32Config: {
        bech32PrefixAccAddr: "cosmos",
        bech32PrefixAccPub: "cosmos" + "pub",
        bech32PrefixValAddr: "cosmos" + "valoper",
        bech32PrefixValPub: "cosmos" + "valoperpub",
        bech32PrefixConsAddr: "cosmos" + "valcons",
        bech32PrefixConsPub: "cosmos" + "valconspub",
    },
    currencies: [ 
        { 
            coinDenom: "ATOM", 
            coinMinimalDenom: "uatom", 
            coinDecimals: 6, 
            coinGeckoId: "cosmos", 
        }, 
    ],
    feeCurrencies: [
        {
            coinDenom: "ATOM",
            coinMinimalDenom: "uatom",
            coinDecimals: 6,
            coinGeckoId: "cosmos",
        },
    ],
    stakeCurrency: {
        coinDenom: "ATOM",
        coinMinimalDenom: "uatom",
        coinDecimals: 6,
        coinGeckoId: "cosmos",
    },
    coinType: 118,
    gasPriceStep: {
        low: 0.01,
        average: 0.025,
        high: 0.03,
    },
});

```

Coin98 supports the basic the `x/bank` module's send feature and balance query. Also, it is able to show the staking reward percentage from the `supply` and `mint` module. (For Stargate chains, Coin98 will find the supply through the `bank` module).
