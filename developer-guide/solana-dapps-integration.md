# Solana DApps Integration

Welcome to Coin98 Extension Wallet Developer Guide. This documentation contains guides for developers to get started developing on Coin98 Extension Wallet

## To detect Coin98 Extension with Solana

To detect whether your browser is running Coin98 Extension, please use:

```javascript
if(window.coin98){
    console.log('Coin98 Extension is installed!');
}
```

Notice: Coin98 Extension Testnet is under development and not available now. The Coin98 Extension on Ethereum JavaScript provider API is specified by [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193). Support `window.coin98.sol`

***

## To connect Coin98 Extension Wallet

To connect Coin98 Extension means to access the user's \[blockchain - like Ethereum] account(s).

```javascript
// Connect & get accounts
window.coin98.sol.request({method: 'sol_accounts'});
// Alias for connection
window.coin98.sol.request({method: 'sol_requestAccounts'});​
//Check if dapp connected
window.coin98.sol.isConnected();
```

***

## To disconnect Coin98 Extension Wallet

To disconnect Coin98 Extension, please use:

```
window.coin98.sol.disconnect()
```

## To experience functions

Once your account is connected, let's start experiencing more functions.‌

### Get Current Account

return `Promise<Array[String]>`

* If wallet can not be found, return `[]` instead of `throw Error`

```javascript
window.coin98.sol.request({ method: 'sol_accounts' }).then(accounts => {
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
window.coin98.sol.request({ method: 'has_wallet', params: ['solana'] })
// Example
window.coin98.sol.request({ method: 'has_wallet', params: ['solana'] }).then(() => {
  // Wallet Exists
}).catch(e => { 
  // Wallet not found
})
```

### Sign Transaction

return: \`Promise<({signature: base58, publicKey: PublicKey})>

```javascript
// Example Sign Transaction
const signature = window.coin98.sol.request({
    method: 'sol_sign',
    params: [<Transaction>]
}).then(({publicKey, signature}) => {
	//Do something with publicKey and signature
});
```

### Verify Signature

return `Promise<boolean>`

```javascript
window.coin98.sol.request({
  method: 'sol_verify',
  params: [signature, msg]
})
```

### RPC Request

return `Promise<Solana RPC>` Currently only support HTTP(s) method Reference: [RPC Method](https://docs.solana.com/developing/clients/jsonrpc-api)

```javascript
window.coin98.sol.request({method: '<Your Method>', params: [args1,....]})
```

***

## To handle events

### List of events

Currently we only support some action event from wallet extension

```javascript
window.coin98.sol.on('event_name', callback);
​//Example
window.coin98.sol.on('close', () => window.location.reload());
window.coin98.sol.on('accountsChanged', () => window.location.reload());
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

## Integration through Wallet Adapter

[https://github.com/solana-labs/wallet-adapter](https://github.com/solana-labs/wallet-adapter)
