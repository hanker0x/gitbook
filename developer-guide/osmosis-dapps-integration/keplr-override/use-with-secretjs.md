# Use with SecretJS

### How to detect Coin98 <a href="#how-to-detect-keplr" id="how-to-detect-keplr"></a>

Coin98 API may be undefined right after the webpage shown. Please check the [How to detect Coin98](https://docs.keplr.app/api/#how-to-detect-keplr) first before reading this section.

### Connecting with SecretJS <a href="#connecting-with-secretjs" id="connecting-with-secretjs"></a>

SecretJS link: [https://www.npmjs.com/package/secretjs (opens new window)](https://www.npmjs.com/package/secretjs)The basics of using SecretJS is similar to CosmJS. Refer to the [Use with CosmJs](https://docs.keplr.app/api/cosmjs) section for more information.

One difference between CosmJS and SecretJS is that we recommend using Coin98's `EnigmaUtils`. By using Coin98's `EnigmaUtils`, you can use Coin98 to encrypt/decrypt, and the decrypted transaction messages are shown to the user in a human-readable format.

```
// Enabling before using the Coin98 is recommended.
// This method will ask the user whether or not to allow access if they haven't visited this website.
// Also, it will request user to unlock the wallet if the wallet is locked.
await window.keplr.enable(chainId);

const offlineSigner = window.getOfflineSigner(chainId);
**const enigmaUtils = window.getEnigmaUtils(chainId);**

// You can get the address/public keys by `getAccounts` method.
// It can return the array of address/public key.
// But, currently, Coin98 extension manages only one address/public key pair.
// XXX: This line is needed to set the sender address for SigningCosmosClient.
const accounts = await offlineSigner.getAccounts();

// Initialize the gaia api with the offline signer that is injected by Coin98 extension.
const cosmJS = new SigningCosmWasmClient(
    "https://lcd-secret.keplr.app/rest",
    accounts[0].address,
    offlineSigner,
    **enigmaUtils**
);
```

#### Suggest Adding SNIP-20 Tokens to Coin98 <a href="#suggest-adding-snip-20-tokens-to-keplr" id="suggest-adding-snip-20-tokens-to-keplr"></a>

```
async suggestToken(chainId: string, contractAddress: string): Promise<void>

```

The webpage can request the user permission to add a SNIP-20 token to Coin98's token list. Will throw an error if the user rejects the request. If a SNIP-20 with the same contract address already exists, nothing will happen.

#### Get SNIP-20 Viewing Key <a href="#get-snip-20-viewing-key" id="get-snip-20-viewing-key"></a>

```
getSecret20ViewingKey(
    chainId: string,
    contractAddress: string
): Promise<string>;

```

Returns the viewing key of a SNIP-20 token registered in Coin98. If the SNIP-20 of the contract address doesn't exist, it will throw an error.

#### Interaction Options <a href="#interaction-options" id="interaction-options"></a>

You can use Coin98 native API’s to set interaction options even when using SecretJS. Please refer to [this section](https://docs.keplr.app/api/#interaction-options).
