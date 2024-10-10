# EVM-compatible DApps Integration

This document guides user how to integrate EVM-compatible Blockchains for DApps. This will apply to all the EVM-compatible Blockchains currently supported in Coin98 Wallet.

## Supported EVM-compatible Blockchains

1. Ethereum
2. Viction
3. BNB Smart Chain
4. Polygon
5. Avalanche C-Chain
6. Arbitrum
7. Aurora
8. Base
9. Bittorrent Chain
10. Boba Network
11. Celo
12. Chiliz
13. Conflux
14. Core
15. Cronos
16. Ethereum PoW
17. f(x) Core EVM
18. Fantom
19. GateChain
20. Gnosis Chain
21. Harmony
22. HECO Chain
23. Humanode
24. KardiaChain
25. Klaytn
26. Kroma
27. KuCoin Commnunity Chain
28. Linea
29. Mantle
30. Moonbeam
31. Nautilus
32. Oasis Network
33. OKXChain
34. opBNB
35. Optimism
36. PlatON Network
37. Polygon zkEVM
38. Ronin
39. Scroll
40. StarkNet Theta Network
41. Theta Network EVM
42. zkSync Era
43. Ancient8 (Testnet)
44. Base (Testnet)
45. Taiko Jolnir (Testnet)
46. Mantle (Testnet)
47. Scroll (Testnet)
48. ZetaChain (Testnet)

## To detect Coin98 Wallet with EVM-compatible Blockchains

To detect whether your browser is running Coin98 Wallet, please use:

```javascript
if(window.coin98 || window.ethereum || window.ethereum?.isCoin98){
    // handle error here
}
```

**Notice**: The Coin98 Wallet on EVM JavaScript provider API is specified by [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193) and [EIP-6963](https://eips.ethereum.org/EIPS/eip-6963). Support `window.ethereum` or `window.coin98.provider` only (`window.web3`is  unsupported)

***

## To connect Coin98 Wallet

To connect Coin98 Wallet means to access the user's \[blockchain - like Ethereum] account(s).

```javascript
// Connect & get accounts
window.coin98.provider.request({method: 'eth_accounts'});
// Alias for connection
window.coin98.provider.request({method: 'eth_requestAccounts'});​
//Check if dapp connected
window.coin98.provider.isConnected();
//Check if the caller's current permissions
window.coin98.provider.request({method: 'wallet_getPermissions'});
//Check if request the given permissions 
window.coin98.provider.request({method: 'wallet_requestPermissions'});
```

***

## To disconnect Coin98 Wallet

To disconnect Coin98 Wallet, please use:

```
window.coin98.provider.disconnect()
```

## To experience functions

Once your account is connected, let's start experiencing more functions.‌

### Get Current Account

return `Promise<Array[String]>`

* If wallet can not be found, return `[]` instead of `throw Error`

```javascript
window.coin98.provider.request({ method: 'eth_accounts' }).then(accounts => {
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
window.coin98.provider.request({ method: 'has_wallet', params: ['ethereum'] })
// Example
window.coin98.provider.request({ method: 'has_wallet', params: ['ethereum'] }).then(() => {
  // Wallet Exists
}).catch(e => { 
  // Wallet not found
})
```

### Sign Transaction

return: `Promise<Signature | RPC: 2.0>`

```javascript
// Example Sign Transactionconst
const signature = window.coin98.provider.request({
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
window.coin98.provider.request({
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
window.coin98.provider.request({
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

return `Promise<string>`- The public encryption key of the Ethereum account whose encryption key should be retrieved

```javascript
let encryptionPublicKey
window.coin98.provider.request({
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

### Add EVM-compatible Chain

result - if the request was successful, and an error otherwise.

```javascript
window.coin98.provider.request
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

Result - if the request was successful, and an error otherwise.

```javascript
try{
	await window.await ethereum.request({
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

Result - true if the token was added, false otherwise

```javascript
window.coin98.provider.request({
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
window.coin98.provider.request({method: '<Your Method>', params: [args1,....]})
```

## Experimental MultiChain Connection (under maintenance)

You can connect and receive multiChain address at the same time by using the following methods

```javascript
async function connect(){
	if(!window.coin98){
		throw new Error('Coin98 Wallet is required');
	}
	const accounts = await window.coin98.connect([<chain 1>, <chain 2>, ...]);
	
	if(!accounts){
		throw new Error('Connect Error | User cancel your request');
	}
	
	// Do anything with accounts:[<address of chain 1>, <address of chain 2>]
}
```

When your connection is successful, chain's properties will be available for your next request. For example:

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

Support subscribe using JSON-RPC notifications. This allows clients to wait for events instead of polling for them. All results will be released at `data` event.

### Methods

```javascript
// For Subscribe
window.coin98.provider.request({
	method: 'eth_subscribe',
	params: ['<type>','<options>']
})
// Its result will be subscription ID which can be used for unsubscribe
// For Unsubscribe
window.coin98.provider.request({
	method: 'eth_unsubscribe',
	params: ['<Subscription ID>']
});
```

### Example

```javascript
// Subscribe for event
const subscriptionID = window.coin98.provider.request({
    method: 'eth_subscribe',
	params: ["logs", 
	{
		address: "0x8320fe7702b96808f7bbc0d4a888ed1468216cfd", 
		topics: ["0xd78a0cb8bb633d06981248b816e7bd33c2a35a6089241d099fa519e361cab902"]}]
})
// You can listen for incoming notifications by
window.coin98.provider.on("data", data => {
	// Do the rest of your work with data
})
```

***

## To handle events

### List of events

Currently we only support some action events from Wallet

```javascript
window.coin98.provider.on('event_name', callback);
​//Example
window.coin98.provider.on('close', () => window.location.reload());
window.coin98.provider.on('accountsChanged', () => window.location.reload());
```

| Events          | Trigger                                       |
| --------------- | --------------------------------------------- |
| accountsChanged | Receive when active account changed in Wallet |
| networkChanged  | Receive when active network changed in Wallet |
| chainChanged    | Receive when active chain changed in Wallet   |
| disconnect      | Receive when disconnecting from Wallet        |
| close           | Alias for disconnect event                    |

{% hint style="warning" %}
**IMPORTANT**: We strongly recommend reloading the page upon **chainChanged**, unless you have a good reason not to
{% endhint %}

| Method               | Description           |
| -------------------- | --------------------- |
| on(event, callback)  | Add event listener    |
| off(event, callback) | Remove event listener |
