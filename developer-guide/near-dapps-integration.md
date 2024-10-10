# Near DApps Integration

Welcome to Coin98 Extension Wallet Developer Guide. This documentation contains guides for developers to get started developing on Coin98 Extension Wallet.‌

## To detect Coin98 Extension with Near

To detect whether your browser is running Coin98 Extension, please use:

```javascript
if(window.coin98 || window.coin98.near || window.near?.isCoin98){
    console.log('Coin98 Extension is installed!');›
}
```

Notice: Coin98 Extension Testnet is under development and not available now.

Support `window.near only` and removal `window.web3`

***

## To connect Coin98 Extension Wallet

To connect Coin98 Extension means to access the user's \[blockchain - like Near] account(s).

```javascript
// Connect DApps
window.coin98.near.connect(<Prefix: optional>,<Contract ID: optional>);
// Get Account
window.coin98.near.selectedAddress;
// Get Full Accounts State
window.coin98.near.request({ method: 'near_accountState' });
// Check if DApps connected
window.coin98.near.isConnected();
```

***

## To disconnect Coin98 Extension Wallet

To disconnect Coin98 Extension, please use:

```
window.coin98.near.disconnect()
```

## To experience functions

Once your account is connected, let's start experiencing more functions.‌

### Call View Method

```javascript
const params = {
    contractId: 'token.v2.ref-finance.near',
    method: 'ft_balance_of',
    args: {
        account_id: account
    }
}
const balance = await window.coin98?.near.request({method: 'near_view', params})
console.log("Your near wallet balance ", balance)
```

### SignAndSend Transaction

```javascript
const params = {
    transactions,
    receiver: account
}

const result = await window.coin98?.near.request({method: 'near_signAndSendTransaction', params})
const balance = await window.coin98?.near.request({method: 'near_view', params})
console.log("Your near wallet balance ", balance)
```
