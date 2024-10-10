# Ronin DApps Integration

Welcome to Coin98 Extension Wallet Developer Guide. This documentation contains guides for developers to get started developing on Coin98 Extension Wallet.‌

## To detect Coin98 Extension with Ronin

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
```

***

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
window.ethereum.request({ method: 'has_wallet', params: ['ronin'] })
// Example
window.ethereum.request({ method: 'has_wallet', params: ['ronin'] }).then(() => {
  // Wallet Exists
}).catch(e => { 
  // Wallet not found
})
```

### Sign

return: `Promise<Signature | RPC: 2.0>`

```javascript
// Example Sign 
const signature = window.ethereum.request({
    method: 'eth_sign',
    params: [
     {
        "to": "string",
        "gas": "string",
        "gasPrice": "string",
        "value": "string",
        "data": "string",
        "nonce": "string"
     }
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
      'to': 'string',
      'gas': 'string',
      'gasPrice': 'string',
      'value': 'string',
      'data': 'string',
      'nonce': 'string'
    }
  ]
})
```

### RPC Request

return `Promise<Ethereum RPC>` Currently only support HTTP(s) method Reference: [RPC Method](https://github.com/axieinfinity/ronin-smart-contracts/tree/master/test/chain)

```javascript
window.ethereum.request({method: '<Your Method>', params: [args1,....]})
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
