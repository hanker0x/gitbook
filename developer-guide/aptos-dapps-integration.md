---
description: (Require app ver > 12.5.0)
---

# Aptos Dapps Integration

### Aptos testnet[​](https://aptos.dev/guides/getting-started/#aptos-testnet) <a href="#aptos-testnet" id="aptos-testnet"></a>

* **REST API Open API spec**: [https://fullnode.testnet.aptoslabs.com/v1/spec#/](https://fullnode.testnet.aptoslabs.com/v1/spec#/)
* **REST service:** [https://fullnode.testnet.aptoslabs.com/v1](https://fullnode.testnet.aptoslabs.com/v1)
* **Faucet service:** [https://faucet.testnet.aptoslabs.com](https://faucet.testnet.aptoslabs.com/)
* **Genesis**: [https://testnet.aptoslabs.com/genesis.blob](https://testnet.aptoslabs.com/genesis.blob)
* **Genesis and waypoint**: [https://github.com/aptos-labs/aptos-genesis-waypoint/tree/main/testnet](https://github.com/aptos-labs/aptos-genesis-waypoint/tree/main/testnet)
* **ChainID**: [Click here to see it on the Aptos Explorer](https://explorer.aptoslabs.com/?network=testnet).

### Aptos devnet[​](https://aptos.dev/guides/getting-started/#aptos-devnet) <a href="#aptos-devnet" id="aptos-devnet"></a>

* **REST API Open API spec**: [https://fullnode.devnet.aptoslabs.com/v1/spec#/](https://fullnode.devnet.aptoslabs.com/v1/spec#/)
* **REST service:** [https://fullnode.devnet.aptoslabs.com/v1](https://fullnode.devnet.aptoslabs.com/v1)
* **Faucet service:** [https://faucet.devnet.aptoslabs.com](https://faucet.devnet.aptoslabs.com/)
* **Genesis**: [https://devnet.aptoslabs.com/genesis.blob](https://devnet.aptoslabs.com/genesis.blob)
* **Waypoint**: [https://devnet.aptoslabs.com/waypoint.txt](https://devnet.aptoslabs.com/waypoint.txt)
* **ChainID**: [Click here to see it on the Aptos Explorer](https://explorer.aptoslabs.com/?network=devnet).

## Dapp API

[**Forum post with discussion**](https://forum.aptoslabs.com/t/wallet-dapp-api-standards/11765/33) There will be some apis that certain wallets may add but there should be a few apis that are standard across wallets. This will make mass adoption easier and will make dapp developers' lives easier.

* `connect({`network: 'mainnet' | 'testnet' | 'devnet'}`)`, `disconnect()`, and `isConnected()`
* `account()`
* `signAndSubmitTransaction(transaction: EntryFunctionPayload)`
* `signMessage(payload: SignMessagePayload)`
* Event listening (`onAccountChanged(listener)`, `onNetworkChanged(listener)`)

```javascript
// Common Args and Responses

interface PublicAccount {
    string address;
    string publicKey;
}

// The important thing to return here is the transaction hash the dapp can wait for it
type [PendingTransaction](https://github.com/aptos-labs/aptos-core/blob/1bc5fd1f5eeaebd2ef291ac741c0f5d6f75ddaef/ecosystem/typescript/sdk/src/generated/models/PendingTransaction.ts)

type [EntryFunctionPayload](https://github.com/aptos-labs/aptos-core/blob/1bc5fd1f5eeaebd2ef291ac741c0f5d6f75ddaef/ecosystem/typescript/sdk/src/generated/models/EntryFunctionPayload.t

```

### connect(), disconnect(), isConnected()[​](https://aptos.dev/guides/building-your-own-wallet/#connect-disconnect-isconnected) <a href="#connect-disconnect-isconnected" id="connect-disconnect-isconnected"></a>

It is important that dapps, aren't allow to send requests to the wallet until the user acknowledges that they want to see these requests.

* `connect({`network: 'mainnet' | 'testnet' | 'devnet'}`)` will prompt the user
  * return `Promise<PublicAccount>`
* `disconnect()` allows the user to stop giving access to a dapp and also helps the dapp with state management
  * return `Promise<void>`
* `isConnected()` able to make requests to the wallet to get current state of connection
  * return `Promise<boolean>`

### account()[​](https://aptos.dev/guides/building-your-own-wallet/#account) <a href="#account" id="account"></a>

**Needs to be connected** The dapp may want to query for the current connected account to get the address or public key.

* `account()` no prompt to the user
  * returns `Promise<PublicAccount>`

### signAndSubmitTransaction(transaction: EntryFunctionPayload)[​](https://aptos.dev/guides/building-your-own-wallet/#signandsubmittransactiontransaction-entryfunctionpayload) <a href="#signandsubmittransactiontransaction-entryfunctionpayload" id="signandsubmittransactiontransaction-entryfunctionpayload"></a>

We will be generate a transaction from payload(simple JSON) using the [sdk](https://github.com/aptos-labs/aptos-core/blob/1bc5fd1f5eeaebd2ef291ac741c0f5d6f75ddaef/ecosystem/typescript/sdk/src/aptos\_client.ts#L217-L221) and then sign and submit it to the wallet's node.

* `signAndSubmitTransaction(transaction: EntryFunctionPayload)` will prompt the user with the transaction they are signing
  * returns `Promise<PendingTransaction>`

### signMessage(payload: SignMessagePayload)[​](https://aptos.dev/guides/building-your-own-wallet/#signmessagepayload-signmessagepayload) <a href="#signmessagepayload-signmessagepayload" id="signmessagepayload-signmessagepayload"></a>

The most common usecase for this function is to verify identity, but there are a few other possible use cases. You may notice some wallets from other chains just provide an interface to sign arbitrary strings. This can be susceptible to man-in-the-middle attacks, signing string transactions, etc.

Types:

```typescript
export interface SignMessagePayload {
  address?: boolean; // Should we include the address of the account in the message
  application?: boolean; // Should we include the domain of the dapp
  chainId?: boolean; // Should we include the current chain id the wallet is connected to
  message: string; // The message to be signed and displayed to the user
  nonce: string; // A nonce the dapp should generate
}

export interface SignMessageResponse {
  address: string;
  application: string;
  chainId: number;
  fullMessage: string; // The message that was generated to sign
  message: string; // The message passed in by the user
  nonce: string,
  prefix: string, // Should always be APTOS
  signature: string; // The signed full message
}
```

* `signMessage(payload: SignMessagePayload)` prompts the user with the `payload.message` to be signed
  * returns `Promise<SignMessageResponse>`

An example: `signMessage({nonce: 1234034, message: "Welcome to dapp!", address: true, application: true, chainId: true })`

This would generate the `fullMessage` to be signed and returned as the `signature`:

```typescript
APTOS
address: 0x000001
chain_id: 7
application: badsite.firebase.google.com
nonce: 1234034
message: Welcome to dapp!
```

### dApp Integration <a href="#step-3-dapp-integration" id="step-3-dapp-integration"></a>

dApps can make requests to the wallet from their website:

* `connect()`: prompts the user to allow connection from the dApp (_neccessary to make other requests_)
* `isConnected()`: returns if the dApp has established a connection with the wallet
* `account()`: gets the address of the account signed into the wallet
* `signAndSubmitTransaction(transaction)`: signs the given transaction and submits to chain
* `signTransaction(transaction)`: signs the given transaction and returns it to be submitted by the dApp
* `disconnect()`: Removes connection between dApp and wallet. Useful when the user wants to remove the connection.&#x20;

<pre class="language-javascript"><code class="lang-javascript">// import transaction build from aptos sdk: https://github.com/aptos-labs/aptos-core/tree/main/ecosystem/typescript/sdk
import { BCS, TxnBuilderTypes } from 'aptos';

// Establish connection to the wallet
const result = await window.aptos.connect()
// OR
const result = await window.coin98.aptos.isConnected()

// Check connection status of wallet
const status = await window.aptos.isConnected()
// OR
<strong>const status = await window.coin98.aptos.isConnected()
</strong>
// Gets the address of the account signed into the wallet
const accountAddress = await window.aptos.account()
// OR
const accountAddress = await window.coin98.aptos.account()

// Create a transaction
const transaction = {
    arguments: [address, '717'],
    function: '0x1::coin::transfer',
    type: 'entry_function_payload',
    type_arguments: ['0x1::aptos_coin::AptosCoin'],
};

// Send transaction to the extension to be signed and submitted to chain
const response = await window.aptos.signAndSubmitTransaction(transaction)
// OR
const response = await window.coin98.aptos.signAndSubmitTransaction(transaction)

// Send transaction to the extension to be signed and returns
const signedTransaction = await window.aptos.signTransaction(transaction)
// OR
const signedTransaction = await window.coin98.aptos.signTransaction(transaction)

// Disconnect dApp from the wallet
await window.aptos.disconnect(transaction)
// OR
await window.coin98.aptos.disconnect(transaction)


</code></pre>

### Verify signature:

```javascript
const signMessage = async () => {
  const result = await window.aptos.signMessage({
      nonce: 3885,
      message: 'Click to sign in and accept the BlueMove Terms of Service. This request will not cost any gas fees.',
      address: true,
      application: true,
      chainId: true
    })
    setState(state => ({ ...state, message: result }))
  }

  const verifyMessage = () => {
    const { message, account } = state
    const sig = message.signature
    const pub = account.publicKey
    const fullMessage = message.fullMessage
    const signature = Buffer.from(sig.substring(2), 'hex')
    const pubKey = Buffer.from(pub.substring(2), 'hex')
    const verify = Nacl.sign.detached.verify(Buffer.from(fullMessage), signature, pubKey)

    alert(verify)
  }
```
