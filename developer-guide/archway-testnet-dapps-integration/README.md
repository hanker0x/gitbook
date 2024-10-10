# Archway (Testnet) DApps Integration

Welcome to Coin98 Extension Wallet Developer Guide. This documentation contains guides for developers to get started developing on Coin98 Extension Wallet.‌

## To detect Coin98 Extension with Cosmos Base Chain

To detect whether your browser is running Coin98 Extension, please use:

```javascript
if (window.coin98) {
  console.log('Coin98 Extension is installed!');
}
```

## Notice: Coin98 Extension Testnet is under development and not available now.

## To connect Coin98 Extension Wallet

To connect Coin98 Extension means to access the user's \[blockchain - like Cosmos] account(s).

```javascript
// Connect only
const connection = window.coin98.cosmos('<chain: String>');
// Connect & get accounts
connection.request({ method: 'cosmos_accounts' });
// Check whether dapp connected or not
connection.isConnected();
```

Currently chains support: \['terra','cosmos']

***

## To disconnect Coin98 Extension Wallet

To disconnect Coin98 Extension, please use:

```javascript
connection.disconnect();
```

## To experience functions

Once your account is connected, let's start experiencing more functions.‌

### Get Current Account

return `Promise<Array[String]>`

* If wallet can not be found, return `[]` instead of `throw Error`

```javascript
connection.request({ method: 'cosmos_accounts' }).then((accounts) => {
  if (accounts[0]) {
    // Do something with accounts
  } else {
    // Wallet not found
  }
});
```

### Check wallet whether exists or not

return `Promise<{data: Boolean}>`

```javascript
const hasWallet = connection.request({ method: 'has_wallet', params: ['<chain>'] });
```

### Execute contract function

return: `Promise<Hash>`

```javascript
const signature = connection.request({
  method: 'cosmos_execute',
  params: ['<contractAddres: string>', '<executeMsg: string>'],
});

// Example Execute Contract
let cw20Contract =
  'juno10rktvmllvgctcmhl5vv8kl3mdksukyqf2tdveh8drpn0sppugwwqjzz30z';
let execMsg =
  '{"transfer":{"amount":"1","recipient":"juno1cx4nq77x3unvl2xsa9fmm9drxkexzkjnzwt2y7"}}';

const response = await connection.request({
  method: 'cosmos_execute',
  params: [cw20Contract, execMsg],
});
```

***

## To handle events

### List of events

Currently we only support some action event from wallet extension

```javascript
connection.on('event_name', callback);
​//Example
connection.on('close', () => window.location.reload());
connection.on('accountsChanged', () => window.location.reload());
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
