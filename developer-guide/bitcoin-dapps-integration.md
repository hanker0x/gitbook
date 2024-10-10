# Bitcoin Dapps Integration

Welcome to Coin98 Extension Wallet Developer Guide. This documentation contains guides for developers to get started developing on Coin98 Extension Wallet.‌

## To detect Coin98 Extension with Bitcoin

To detect whether your browser is running Coin98 Extension, please use:

```javascript
if (typeof window.coin98?.bitcoin !== 'undefined') {
  console.log('Coin98 Wallet is installed!');
}
```

***

### Connecting to Coin98 Wallet

"Connecting" or "logging in" to Coin98 Wallet effectively means "to access the user's Bitcoin account(s)".

You should **only** initiate a connection request in response to direct user action, such as clicking a button. You should **always** disable the "connect" button while the connection request is pending. You should **never** initiate a connection request on page load.

We recommend that you provide a button to allow the user to connect Coin98 Wallet to your dapp. Clicking this button should call the following method:

```javascript
coin98.bitcoin.requestAccounts()
```

## Methods

### requestAccounts

```typescript
coin98.bitcoin.requestAccounts()
```

Connect the current account.

**Parameters**

none

**Returns**

`Promise` returns `string[]` : Address of current account.

**Example**

```typescript
try {
  let accounts = await window.coin98?.bitcoin.requestAccounts();
  console.log('connect success', accounts);
} catch (e) {
  console.log('connect failed');
}
> connect success ['tb1qrn7tvhdf6wnh790384ahj56u0xaa0kqgautnnz']
```

### getAccounts

```typescript
coin98.bitcoin.getAccounts()
```

Get address of current account

**Parameters**

none

**Returns**

* `Promise` - `string`: address of current account

**Example**

```typescript
try {
  let res = await window.coin98?.bitcoin.getAccounts();
  console.log(res)
} catch (e) {
  console.log(e);
}
> ["tb1qrn7tvhdf6wnh790384ahj56u0xaa0kqgautnnz"]
```

### getNetwork

```typescript
coin98.bitcoin.getNetwork()
```

get network

**Parameters**

none

**Returns**

* `Promise` - `string`: the network. `livenet` and `testnet`

**Example**

```typescript
try {
  let res = await window.coin98?.bitcoin.getNetwork();
  console.log(res)
} catch (e) {
  console.log(e);
}

> 0
```

### switchNetwork

```typescript
coin98.bitcoin.switchNetwork(network)
```

switch network

**Parameters**

* `network` - `string`: the network. `livenet` and `testnet`

**Returns**

none

**Example**

```typescript
try {
  let res = await window.coin98?.bitcoin.switchNetwork("livenet");
  console.log(res)
} catch (e) {
  console.log(e);
}

> 0
```

### getPublicKey

```typescript
coin98.bitcoin.getPublicKey()
```

Get publicKey of current account.

**Parameters**

none

**Returns**

* `Promise` - `string`: publicKey

**Example**

```typescript
try {
  let res = await window.coin98?.bitcoin.getPublicKey();
  console.log(res)
} catch (e) {
  console.log(e);
}
> 03cbaedc26f03fd3ba02fc936f338e980c9e2172c5e23128877ed46827e935296f
```

### getBalance

```typescript
coin98.bitcoin.getBalance()
```

Get BTC balance

**Parameters**

none

**Returns**

* `Promise` - `Object`:
  * `confirmed` - `number` : the confirmed satoshis
  * `unconfirmed` - `number` : the unconfirmed satoshis
  * `total` - `number` : the total satoshis

**Example**

```typescript
try {
  let res = await window.coin98?.bitcoin.getBalance();
  console.log(res)
} catch (e) {
  console.log(e);
}

> {
    "confirmed":0,
    "unconfirmed":100000,
    "total":100000
  }
```

### sendBitcoin

```typescript
coin98.bitcoin.sendBitcoin(toAddress, satoshis, options)
```

Send BTC

**Parameters**

* `toAddress` - `string`: the address to send
* `satoshis` - `number`: the satoshis to send
* `options` - `object`:  (optional)
  * `feeRate` - `number`: the network fee rate

**Returns**

* `Promise` - `string`: txid

**Example**

```typescript
try {
  let txid = await window.coin98?.bitcoin.sendBitcoin("tb1qrn7tvhdf6wnh790384ahj56u0xaa0kqgautnnz",1000);
  console.log(txid)
} catch (e) {
  console.log(e);
}
```

### signMessage

```typescript
coin98.bitcoin.signMessage(msg[, type])
```

sign message

**Parameters**

* `msg` - `string`: a string to sign
* `type` - `string`:  (Optional) "ecdsa" | "bip322-simple". default is "ecdsa"

**Returns**

* `Promise` - `string`: the signature.

**Example**

```typescript
// sign by ecdsa
try {
  let res = await window.coin98?.bitcoin.signMessage("abcdefghijk123456789");
  console.log(res)
} catch (e) {
  console.log(e);
}

> G+LrYa7T5dUMDgQduAErw+i6ebK4GqTXYVWIDM+snYk7Yc6LdPitmaqM6j+iJOeID1CsMXOJFpVopvPiHBdulkE=

// verify by ecdsa
import { verifyMessage } from "@unisat/wallet-utils";
const pubkey = "026887958bcc4cb6f8c04ea49260f0d10e312c41baf485252953b14724db552aac";
const message = "abcdefghijk123456789";
const signature = "G+LrYa7T5dUMDgQduAErw+i6ebK4GqTXYVWIDM+snYk7Yc6LdPitmaqM6j+iJOeID1CsMXOJFpVopvPiHBdulkE=";
const result = verifyMessage(pubkey,message,signature);
console.log(result);

> true

> AkcwRAIgeHUcjr0jODaR7GMM8cenWnIj0MYdGmmrpGyMoryNSkgCICzVXWrLIKKp5cFtaCTErY7FGNXTFe6kuEofl4G+Vi5wASECaIeVi8xMtvjATqSSYPDRDjEsQbr0hSUpU7FHJNtVKqw=


```

### pushTx

```typescript
coin98.bitcoin.pushTx(options)
```

Push Transaction

**Parameters**

* `options` - `Object`:
  * `rawtx` - `string`: rawtx to push

**Returns**

* `Promise` - `string`: txid

**Example**

```typescript
try {
  let txid = await window.coin98?.bitcoin.pushTx({
    rawtx:"0200000000010135bd7d..."
  });
  console.log(txid)
} catch (e) {
  console.log(e);
}
```

### signPsbt

```typescript
coin98.bitcoin.signPsbt(psbtHex[, options])
```

Sign PSBT

This method will traverse all inputs that match the current address to sign.

**Parameters**

* `psbtHex` - `string`: the hex string of psbt to sign
* options
  * `autoFinalized` - `boolean`: whether finalize psbt after signing, default is true
  * `toSignInputs` - `array`:&#x20;
    * `index` - `number`: which input to sign&#x20;
    * `address` - `string`: (at least specify either an address or a publicKey) Which corresponding private key to use for signing
    * `publicKey` - `string`: (at least specify either an address or a publicKey) Which corresponding private key to use for signing
    * `sighashTypes` - `number[]`: (optionals) sighashTypes

**Returns**

* `Promise` - `string`: the hex string of signed psbt

**Example**

```typescript
try {
  let res = await window.coin98?.bitcoin.signPsbt(
    "70736274ff01007d....",
    {
        autoFinalized:false,
        toSignInputs:[
          {
            index: 0,
            address: "tb1q8h8....mjxzny",
          },
          {
            index: 1,
            publicKey: "tb1q8h8....mjxzny",
            sighashTypes: [1]
          },
          {
            index: 2,
            publicKey: "02062...8779693f",
          }
        ]
    }
  );
  console.log(res)
} catch (e) {
  console.log(e);
}
```

### signPsbts

```typescript
coin98.bitcoin.signPsbts(psbtHexs[, options])
```

Sign Multiple PSBTs at once

This method will traverse all inputs that match the current address to sign.

**Parameters**

* `psbtHexs` - `string[]`: the hex strings of psbt to sign
* `options` - `object`\[]: the options of signing psbt
  * `autoFinalized` - `boolean`: whether finalize psbt after signing, default is true
  * `toSignInputs` - `array`:&#x20;
    * `index` - `number`: which input to sign&#x20;
    * `address` - `string`: (at least specify either an address or a publicKey) Which corresponding private key to use for signing
    * `publicKey` - `string`: (at least specify either an address or a publicKey) Which corresponding private key to use for signing
    * `sighashTypes` - `number[]`: (optionals) sighashTypes

**Returns**

* `Promise` - `string[]`: the hex strings of signed psbt

**Example**

```typescript
try {
  let res = await window.coin98?.bitcoin.signPsbts(["70736274ff01007d...","70736274ff01007d..."]);
  console.log(res)
} catch (e) {
  console.log(e);
}
```

### pushPsbt

```typescript
coin98.bitcoin.pushPsbt(psbtHex)
```

Push transaction

**Parameters**

* `psbtHex` - `string`: the hex string of psbt to push

**Returns**

* `Promise` - `string`: txid

**Example**

```typescript
try {
  let res = await window.coin98?.bitcoin.pushPsbt("70736274ff01007d....");
  console.log(res)
} catch (e) {
  console.log(e);
}
```

## Events

### accountsChanged

```typescript
coin98.bitcoin.on('accountsChanged', handler: (accounts: Array<string>) => void);
coin98.bitcoin.removeListener('accountsChanged', handler: (accounts: Array<string>) => void);
```

The `accountsChanged` will be emitted whenever the user's exposed account address changes.

### networkChanged

```typescript
coin98.bitcoin.on('networkChanged', handler: (network: string) => void);
coin98.bitcoin.removeListener('networkChanged', handler: (network: string) => void);
```

The `networkChanged` will be emitted whenever the user's network changes.
