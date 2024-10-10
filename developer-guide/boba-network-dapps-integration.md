# Boba Network DApps Integration

Welcome to Coin98 Extension Wallet Developer Guide. This documentation contains guides for developers to get started developing on Coin98 Extension Wallet.‌

## To detect Coin98 Extension with Boba Network

To detect whether your browser is running Coin98 Extension, please use:

```javascript
if(window.coin98 || window.ethereum || window.ethereum?.isCoin98){
    console.log('Coin98 Extension is installed!');
}
```

Notice: Coin98 Extension Testnet is under development and not available now. The Coin98 Extension on Ethereum JavaScript provider API is specified by [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193). Support `window.ethereum only` and removal `window.web3`

***

## To connect Coin98 Extension Wallet

To connect Coin98 Extension means to access the user's \[blockchain - like Ethereum] account(s).

```javascript
// Connect & get accounts
window.ethereum.request({method: 'eth_accounts'});
// Alias for connection
window.ethereum.request({method: 'eth_requestAccounts'});​
//Check if dapp connected
window.ethereum.isConnected();
//Check if the caller's current permissions
window.ethereum.request({method: 'wallet_getPermissions'});
//Check if request the given permissions 
window.ethereum.request({method: 'wallet_requestPermissions'});
```

## To disconnect Coin98 Extension Wallet

To disconnect Coin98 Extension, please use:

```
window.ethereum.disconnect()
```

## To experience functions

Once your account is connected, let's start experiencing more functions.‌

### Get Current Account

return `Promise<Array[String]>`

* If wallet can not be found, return `[]` instead of `throw Error`

```javascript
window.ethereum.request({ method: 'eth_accounts' }).then(accounts => {
  if (accounts[0]) {
    // Do something with accounts
  } else {
    // Wallet not found
  }
})
```

### Check wallet whether exists or not

return `Promise<{data: Boolean}>`

```javascript
window.ethereum.request({ method: 'has_wallet', params: ['boba'] })
// Example
window.ethereum.request({ method: 'has_wallet', params: ['boba'] }).then(() => {
  // Wallet Exists
}).catch(e => { 
  // Wallet not found
})
```

### Sign Transaction

return: `Promise<Signature | RPC: 2.0>`

```javascript
// Example Sign Transactionconst
const signature = window.ethereum.request({
    method: 'eth_signTransaction',
    params: [
        "from": "string",
        "to": "string",
        "gas": "string",
        "gasPrice": "string",
        "value": "string",
        "data": "string",
        "nonce": "string"
    ]
});
```

### Transfer

return `Promise<hash>`

```javascript
window.ethereum.request({
  method: 'eth_sendTransaction',
  params: [
    {
      from: 'string',
      to: 'string',
      gas: 'string',
      gasPrice: 'string',
      value: 'string',
      data: 'string',
      nonce: 'string'
    }
  ]
})
```

### Decrypt

return `Promise<string>`

```javascript
window.ethereum.request({
  method: 'eth_decrypt',
  params: [encryptedMessage, accounts[0]],
  })
   .then((decryptedMessage) =>
    console.log('The decrypted message is:', decryptedMessage)
  )
  .catch((error) => console.log(error.message));
})
```

### Get Encryption Public Key

return `Promise<string>`- The public encryption key of the Ethereum account whose encryption key should be retrived

```javascript
let encryptionPublicKey
window.ethereum.request({
  method: 'eth_getEncryptionPublicKey',
  params: [accounts[0]], // you must have access to the specified account
  })
  .then((result) => {
    encryptionPublicKey = result;
  })
  .catch((error) => {
    if (error.code === 4001) {
      // EIP-1193 userRejectedRequest error
      console.log("We can't encrypt anything without the key.");
    } else {
      console.error(error);
    }
  });
```

### Encrypt

```javascript
const ethUtil = require('ethereumjs-util');
const encryptedMessage = ethUtil.bufferToHex(
  Buffer.from(
    JSON.stringify(
      sigUtil.encrypt(
        {
          publicKey: encryptionPublicKey,
          data: 'hello world!,
          version: 'x25519-xsalsa20-poly1305',
        }
      )
    ),
    'utf8'
  )
);
```

### Add Ethereum Chain

Return null - if the request was successful, and an error otherwise.

```javascript
window.ethereum.request
interface AddEthereumChainParameter {
  chainId: string; // A 0x-prefixed hexadecimal string
  chainName: string;
  nativeCurrency: {
    name: string;
    symbol: string; // 2-6 characters long
    decimals: 18;
  };
  rpcUrls: string[];
  blockExplorerUrls?: string[];
  iconUrls?: string[]; // Currently ignored.
}
```

### Switch Ethereum Chain

Return null - if the request was successful, and an error otherwise.

```javascript
window.await ethereum.request({
method: 'wallet_switchEthereumChain',
    params: [{ chainId: '0xf00' }],
  });
} catch (switchError) {
  // This error code indicates that the chain has not been added to Coin98.
  if (switchError.code === 4902) {
    try {
      await ethereum.request({
        method: 'wallet_addEthereumChain',
        params: [
          {
            chainId: '0xf00',
            chainName: '...',
            rpcUrls: ['https://...'] /* ... */,
          },
        ],
      });
    } catch (addError) {
      // handle "add" error
    }
  }
  // handle other "switch" errors
}
```

### Watch Asset

Return Boolean - true if the token was added, fasle otherwise

```javascript
window.ethereum.request({
method: 'wallet_watchAsset',
    params: {
      type: 'ERC20',
      options: {
        address: '0xb60e8dd61c5d32be8058bb8eb970870f07233155',
        symbol: 'FOO',
        decimals: 18,
        image: 'https://foo.io/token-image.svg',
      },
    },
  })
  .then((success) => {
    if (success) {
      console.log('FOO successfully added to wallet!');
    } else {
      throw new Error('Something went wrong.');
    }
  })
  .catch(console.error);
```

### RPC Request

return `Promise<Ethereum RPC>` Currently only support HTTP(s) method Reference: [RPC Method](http://google.com)

```javascript
window.ethereum.request({method: '<Your Method>', params: [args1,....]})
```

## Experimental MultiChain Connection

You can connect and receive multiChain address at the same time by using the following methods

```javascript
async function connect(){
	if(!window.coin98){
		throw new Error('Coin98 Extension is required');
	}
	const accounts = await window.coin98.connect([<chain 1>, <chain 2>, ...]);
	
	if(!accounts){
		throw new Error('Connect Error | User cancel your request');
	}
	
	// Do anything with accounts:[<address of chain 1>, <address of chain 2>]
}
```

When your connection is success, chain's properties will be available for your next request. For example:

```javascript
// For example | connect ether & solana
async function connect(){
	const conn = await window.coin98.connect(['ether','solana']);
	// If user accept the request, those properties will available at window.coin98;
	//  window.solana || Checkout document of solana connection guide for more methods;
	//	window.ether || Checkout document of ether connection guide for more methods;
}
```

Chain's Name can be found at

```javascript
const { CHAIN_NAME } = window.coin98
```

## Subscription

Support subscribe using JSON-RPC notifications. This allows clients to wait for events instead of polling for them. All result will be release at `data` event.

#### Methods

```javascript
// For Subscribe
window.ethereum.request({
	method: 'eth_subscribe',
	params: ['<type>','<options>']
})
// Its result will be subscription ID which can be used for unsubscribe
// For Unsubscribe
window.ethereum.request({
	method: 'eth_unsubscribe',
	params: ['<Subscription ID>']
});
```

#### Example

```javascript
// Subscribe for event
const subscriptionID = window.ethereum.request({
    method: 'eth_subscribe',
	params: ["logs", 
	{
		address: "0x8320fe7702b96808f7bbc0d4a888ed1468216cfd", 
		topics: ["0xd78a0cb8bb633d06981248b816e7bd33c2a35a6089241d099fa519e361cab902"]}]
})
// You can listen for incoming notifications by
window.ethereum.on("data", data => {
	// Do the rest of your work with data
})
```

***

## To handle events

### List of events

Currently we only support some action event from wallet extension

```javascript
window.ethereum.on('event_name', callback);
​//Example
window.ethereum.on('close', () => window.location.reload());
window.ethereum.on('accountsChanged', () => window.location.reload());
```

| Events          | Trigger                                          |
| --------------- | ------------------------------------------------ |
| accountsChanged | Receive when active account changed in Extension |
| networkChanged  | Receive when active network changed in Extension |
| chainChanged    | Receive when active chain changed in Extension   |
| disconnect      | Receive when disconnect from Extension           |
| close           | Alias for disconnect event                       |

| Method               | Description           |
| -------------------- | --------------------- |
| on(event, callback)  | Add event listener    |
| off(event, callback) | Remove event listener |
