# Developer Guide

Welcome to Ancient8 Extension Wallet Developer Guide. This documentation contains guides for developers to get started developing on Ancient8 Extension Wallet.‌

## Connect

### The easiest way to connect to Ancient8 Wallet

Check if the provider is window.ancient8, if not, please replace it with the exclusive Ancient8 Wallet provider window.ancient8.

For example, see below:

```
function getProvider() {
  const provider = window.ancient8;
  if (!provider) {
    return window.open('https://ancient8.gg/');
  }
  return provider;
}
```

### To detect Ancient8 Wallet Extension

```
if(window.ancient8 ){
    console.log('Ancient8 Extension is installed!');
}
```

### To connect Ancient8 Extension Wallet

To connect Ancient8 Extension means to access the user's \[blockchain - like Ethereum] account(s).

***

#### Access a user's accounts

We recommend providing a button to allow users to connect Ancient8 Wallet to your dapp. Selecting this button should call eth\_requestAccounts to access the user's accounts.

```
//Alias for connection
window.ancient8.request({method: 'eth_requestAccounts'});​
```

#### Handle accounts[​](https://docs.metamask.io/wallet/how-to/connect/access-accounts/#handle-accounts)

Use the eth\_accounts RPC method to handle user accounts. Listen to the accountsChanged provider event to be notified when the user changes accounts.

```
// Connect & get accounts
window.ancient8.request({method: 'eth_accounts'});
```

#### To check if Dapp connected

```
window.ancient8.isConnected();
```

### To disconnect Ancient8 Extension Wallet

To disconnect Ancient8 Extension, please use:

```
window.ancient8.disconnect()
```

## To experience functions

Once your account is connected, let's start experiencing more functions.‌

### Account

#### How to get Current Account

return Promise\<Array\[String]>

* If wallet can not be found, return \[] instead of throw Error

```
window.ancient8.request({ method: 'eth_accounts' }).then(accounts => {
  if (accounts[0]) {
    // Do something with accounts
  } else {
    // Wallet not found
  }
})
```

### Sign Transaction

**Important**

eth\_signTypedData\_v1, and eth\_signTypedData\_v3 are deprecated. We highly recommend user to use eth\_signTypedData\_v4 or personal\_sign.

#### Personal\_sign

personal\_sign provides a simple means to request signatures that are human-readable and don't require efficient processing on-chain. It's commonly used for signature challenges authenticated on a web server.

Ancient8 Wallet implements both personal\_sign and eth\_sign. You might need to check what method your supported signers use for a given implementation.

```
method: 'personal_sign',
params: [msg, from],
```

#### eth\_signTypedData\_v4

eth\_signTypedData\_v4 offers highly legible signatures that can be processed efficiently on-chain. Adheres to the [EIP-712 ](https://eips.ethereum.org/EIPS/eip-712)standard, enabling users to sign sign typed structured data that can be confirmed on-chain.

```
  const msgParams = JSON.stringify({
    domain: {
      chainId: 0x58,
      name: 'Ether Mail',
      verifyingContract: '0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC',
      version: '1',
    },

    message: {
      contents: 'Hello, Bob!',
      attachedMoneyInEth: 4.2,
      from: {
        name: 'Cow',
        wallets: [
          '0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826',
          '0xDeaDbeefdEAdbeefdEadbEEFdeadbeEFdEaDbeeF',
        ],
      },
      to: [
        {
          name: 'Bob',
          wallets: [
            '0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB',
            '0xB0BdaBea57B0BDABeA57b0bdABEA57b0BDabEa57',
            '0xB0B0b0b0b0b0B000000000000000000000000000',
          ],
        },
      ],
    },
    
    primaryType: 'Mail',
    types: {
      EIP712Domain: [
        { name: 'name', type: 'string' },
        { name: 'version', type: 'string' },
        { name: 'chainId', type: 'uint256' },
        { name: 'verifyingContract', type: 'address' },
      ],
      Group: [
        { name: 'name', type: 'string' },
        { name: 'members', type: 'Person[]' },
      ],
      Mail: [
        { name: 'from', type: 'Person' },
        { name: 'to', type: 'Person[]' },
        { name: 'contents', type: 'string' },
      ],
      Person: [
        { name: 'name', type: 'string' },
        { name: 'wallets', type: 'address[]' },
      ],
    },
  ;
```

**Note:**

Gas limit​ : is an optional parameter, since Ancient8 Wallet automatically calculates a reasonable gas price.

chainid: The chain ID is derived from the user's current selected network at window.ancient8.net\_version.

#### personal\_ecRecover

personal\_ecRecover returns the address associated with the private key that was used to calculate a signature.

```
method: 'personal_ecRecover',
params: [message, signature],
```

### Transfer

This method requires that the user has granted permission to interact with their account first, so please make sure to call eth\_requestAccounts or wallet\_requestPermissions first.

#### eth\_sendTransaction

return Promise\<hash>

```
window.Ancient8.request({
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

### Decrypt & Encrypt

#### eth\_decrypt

Requests that Ancient8 Wallet decrypt the specified encrypted message.

* The message must have been encrypted using the public encryption key of the specified Ethereum address.

return Promise\<string>

```
window.ancient8.request({
  method: 'eth_decrypt',
  params: [encryptedMessage, accounts[0]],
  })
   .then((decryptedMessage) =>
    console.log('The decrypted message is:', decryptedMessage)
  )
  .catch((error) => console.log(error.message));
})
```

#### eth\_getEncryptionPublicKey

Requests that the user share their public encryption key. Returns a public encryption key, or rejects if the user denies the request.

return Promise\<string>- The public encryption key of the Ethereum account whose encryption key should be retrived

```
let encryptionPublicKey
window.ancient8.request({
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

#### Encrypt

```
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

#### List of events

Currently we only support some action event from wallet extension

```
window.ethereum.on('event_name', callback);

​//Example

window.ancient8.on('close', () => window.location.reload());

window.ancient8.on('accountsChanged', () => window.location.reload());
```

| Events          | Trigger                                          |
| --------------- | ------------------------------------------------ |
| accountsChanged | Receive when active account changed in Extension |
| networkChanged  | Receive when active network changed in Extension |

| Method               | Description           |
| -------------------- | --------------------- |
| on(event, callback)  | Add event listener    |
| off(event, callback) | Remove event listener |
