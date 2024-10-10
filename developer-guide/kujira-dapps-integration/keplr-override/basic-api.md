# Basic API

### Note: Please create cosmos wallet before using this function How to detect Coin98 <a href="#how-to-detect-keplr" id="how-to-detect-keplr"></a>

You can determine whether Coin98 is installed on the user device by checking `window.keplr`. If `window.keplr` returns `undefined` after document.load, Coin98 is not installed. There are several ways to wait for the load event to check the status. Refer to the examples below:

You can register the function to `window.onload`:

```
window.onload = async () => {
    if (!window.keplr) {
        alert("Please install keplr extension");
    } else {
        const chainId = "kaiyo-1";

        // Enabling before using the Coin98 is recommended.
        // This method will ask the user whether to allow access if they haven't visited this website.
        // Also, it will request that the user unlock the wallet if the wallet is locked.
        await window.keplr.enable(chainId);
    
        const offlineSigner = window.keplr.getOfflineSigner(chainId);
    
        // You can get the address/public keys by `getAccounts` method.
        // It can return the array of address/public key.
        // But, currently, Coin98 extension manages only one address/public key pair.
        // XXX: This line is needed to set the sender address for SigningCosmosClient.
        const accounts = await offlineSigner.getAccounts();
    
        // Initialize the gaia api with the offline signer that is injected by Coin98 extension.
        const cosmJS = new SigningCosmosClient(
            "https://lcd-cosmoshub.keplr.app",
            accounts[0].address,
            offlineSigner,
        );
    }
}

```

or track the document's ready state through the document event listener:

```
async getKeplr(): Promise<Keplr | undefined> {
    if (window.keplr) {
        return window.keplr;
    }
    
    if (document.readyState === "complete") {
        return window.keplr;
    }
    
    return new Promise((resolve) => {
        const documentStateChange = (event: Event) => {
            if (
                event.target &&
                (event.target as Document).readyState === "complete"
            ) {
                resolve(window.keplr);
                document.removeEventListener("readystatechange", documentStateChange);
            }
        };
        
        document.addEventListener("readystatechange", documentStateChange);
    });
}

```

There may be multiple ways to achieve the same result, and no preferred method.

### &#x20;Coin98-specific features <a href="#keplr-specific-features" id="keplr-specific-features"></a>

If you were able to connect Coin98 with CosmJS, you may skip to the [Use Coin98 with CosmJS](https://docs.keplr.app/api/cosmjs.html) section.

While Coin98 supports an easy way to connect to CosmJS, there are additional functions specific to Coin98 which provides additional features.

#### &#x20;Using with Typescript <a href="#using-with-typescript" id="using-with-typescript"></a>

**`window.d.ts`**\


```
import { Window as KeplrWindow } from "@keplr-wallet/types";

declare global {
  // eslint-disable-next-line @typescript-eslint/no-empty-interface
  interface Window extends KeplrWindow {}
}

```

The `@keplr-wallet/types` package has the type definition related to Keplr.\
If you're using TypeScript, run `npm install --save-dev @keplr-wallet/types` or `yarn add -D @keplr-wallet/types` to install `@keplr-wallet/types`.\
Then, you can add the `@keplr-wallet/types` window to a global window object and register the Coin98 related types.

> Usage of any other packages besides @keplr-wallet/types is not recommended.
>
> * Any other packages besides @keplr-wallet/types are actively being developed, backward compatibility is not in the scope of support.
> * Since there are active changes being made, documentation is not being updated to the most recent version of the package as of right now. Documentations would be updated as packages get stable.

#### Enable Connection <a href="#enable-connection" id="enable-connection"></a>

```
enable(chainIds: string | string[]): Promise<void>
```

The `window.keplr.enable(chainIds)` method requests the extension to be unlocked if it's currently locked. If the user hasn't given permission to the webpage, it will ask the user to give permission for the webpage to access Coin98.

`enable` method can receive one or more chain-id as an array. When the array of chain-id is passed, you can request permissions for all chains that have not yet been authorized at once.

If the user cancels the unlock or rejects the permission, an error will be thrown.

#### &#x20;Get Address / Public Key <a href="#get-address-public-key" id="get-address-public-key"></a>

```
getKey(chainId: string): Promise<{
    // Name of the selected key store.
    name: string;
    algo: string;
    pubKey: Uint8Array;
    address: Uint8Array;
    bech32Address: string;
}>
```

If the webpage has permission and Coin98 is unlocked, this function will return the address and public key in the following format:

```
{
    // Name of the selected key store.
    name: string;
    algo: string;
    pubKey: Uint8Array;
    address: Uint8Array;
    bech32Address: string;
    isNanoLedger: boolean;
}
```

It also returns the nickname for the key store currently selected, which should allow the webpage to display the current key store selected to the user in a more convenient mane.\
`isNanoLedger` field in the return type is used to indicate whether the selected account is from the Ledger Nano. Because current Cosmos app in the Ledger Nano doesn't support the direct (protobuf) format msgs, this field can be used to select the amino or direct signer. [Ref](https://docs.keplr.app/api/cosmjs.html#types-of-offline-signers)

#### Sign Amino <a href="#sign-amino" id="sign-amino"></a>

```
signAmino(chainId: string, signer: string, signDoc: StdSignDoc): Promise<AminoSignResponse>
```

Similar to CosmJS `OfflineSigner`'s `signAmino`, but Coin98's `signAmino` takes the chain-id as a required parameter. Signs Amino-encoded `StdSignDoc`.

#### Sign Direct / Protobuf <a href="#sign-direct-protobuf" id="sign-direct-protobuf"></a>

```
signDirect(chainId:string, signer:string, signDoc: {
    /** SignDoc bodyBytes */
    bodyBytes?: Uint8Array | null;

    /** SignDoc authInfoBytes */
    authInfoBytes?: Uint8Array | null;

    /** SignDoc chainId */
    chainId?: string | null;

    /** SignDoc accountNumber */
    accountNumber?: Long | null;
  }): Promise<DirectSignResponse>
```

Similar to CosmJS `OfflineDirectSigner`'s `signDirect`, but Coin98's `signDirect` takes the chain-id as a required parameter. Signs Proto-encoded `StdSignDoc`.

#### Request Transaction Broadcasting <a href="#request-transaction-broadcasting" id="request-transaction-broadcasting"></a>

```
sendTx(
    chainId: string,
    tx: Uint8Array,
    mode: BroadcastMode
): Promise<Uint8Array>;
```

This function requests Coin98 to delegates the broadcasting of the transaction to Coin98's LCD endpoints (rather than the webpage broadcasting the transaction). This method returns the transaction hash if it succeeds to broadcast, if else the method will throw an error. When Coin98 broadcasts the transaction, Coin98 will send the notification on the transaction's progress.

#### Request Signature for Arbitrary Message <a href="#request-signature-for-arbitrary-message" id="request-signature-for-arbitrary-message"></a>

```
signArbitrary(
    chainId: string,
    signer: string,
    data: string | Uint8Array
): Promise<StdSignature>;
verifyArbitrary(
    chainId: string,
    signer: string,
    data: string | Uint8Array,
    signature: StdSignature
): Promise<boolean>;
```

This is an experimental implementation of [ADR-36](https://github.com/cosmos/cosmos-sdk/blob/master/docs/architecture/adr-036-arbitrary-signature.md). Use this feature at your own risk.

It's main usage is to prove ownership of an account off-chain, requesting ADR-36 signature using the `signArbitrary` API.

If requested sign doc with the `signAnimo` API with the ADR-36 that Coin98 requires instead of using the `signArbitary` API, it would function as `signArbitary`

* Only supports sign doc in the format of Amino. (in the case of protobuf, [ADR-36 ](https://github.com/cosmos/cosmos-sdk/blob/master/docs/architecture/adr-036-arbitrary-signature.md)requirements aren't fully specified for implementation)
* sign doc message should be single and the message type should be "sign/MsgSignData"
* sign doc "sign/MsgSignData" message should have "signer" and "data" as its value. "data" should be base64 encoded
* sign doc chain\_id should be an empty string("")
* sign doc memo should be an empty string("")
* sign doc account\_number should be "0"
* sign doc sequence should be "0"
* sign doc fee should be `{gas: "0", amount: []}`

When using the `signArbitrary` API, if the `data` parameter type is `string`, the signature page displays as plain text.

Using `verifyArbitrary`, you can verify the results requested by `signArbitrary` API or `signAmino` API that has been requested with the ADR-36 spec standards.

`verifyArbitrary` has been only implemented for simple usage. `verifyArbitrary` returns the result of the verification of the current selected account's sign doc. If the account is not the currently selected account, it would throw an error.

It is recommended to use `verifyADR36Amino` function in the `@keplr-wallet/cosmos` package or your own implementation instead of using `verifyArbitrary` API.

#### Interaction Options <a href="#interaction-options" id="interaction-options"></a>

```
export interface KeplrIntereactionOptions {
  readonly sign?: KeplrSignOptions;
}

export interface KeplrSignOptions {
  readonly preferNoSetFee?: boolean;
  readonly preferNoSetMemo?: boolean;
}
```

Coin98 extension offers additional options to customize interactions between the frontend website and Coin98 extension.

If `preferNoSetFee` is set to true, Coin98 will prioritize the frontend-suggested fee rather than overriding the tx fee setting of the signing page.

If `preferNoSetMemo` is set to true, Coin98 will not override the memo and set fix memo as the front-end set memo.

You can set the values as follows:

```
window.keplr.defaultOptions = {
    sign: {
        preferNoSetFee: true,
        preferNoSetMemo: true,
    }
}
```

### Custom event <a href="#custom-event" id="custom-event"></a>

#### Change Key Store Event <a href="#change-key-store-event" id="change-key-store-event"></a>

```
keplr_keystorechange
```

When the user switches their key store/account after the webpage has received the information on the key store/account the key that the webpage is aware of may not match the selected key in Coin98 which may cause issues in the interactions.

To prevent this from happening, when the key store/account is changed, Coin98 emits a `keplr_keystorechange` event to the webpage's window. You can request the new key/account based on this event listener.

```
window.addEventListener("keplr_keystorechange", () => {
    console.log("Key store in Keplr is changed. You may need to refetch the account info.")
})
```

