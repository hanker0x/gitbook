# Coin98 Connect Integration

### Introduction <a href="#introduction" id="introduction"></a>

### Getting Started <a href="#getting-started" id="getting-started"></a>

Example Dapps connection requested via Coin98 Connect&#x20;

[https://connect.coin98.com](https://connect.coin98.com/)

Quick Start For Dapps

[NPM Package](https://www.npmjs.com/package/@coin98-com/connect-sdk)

### Install

```bash
npm install --save @coin98-com/connect-sdk
```

### Usage

**Dapps Website Connection Example**

```ts
import { Client, Chain } from '@coin98-com/connect-sdk'

const client = new Client()


client.connect(Chain.fantom, {
  logo: "Provide Your App Logo URL",
  name: "Your App Name",
  url: "Provide Your App URL"
})
```

#### React Native Connection (Without Expo) Example

```ts
import { Client, Chain } from '@coin98-com/connect-sdk/dist/native'

const client = new Client()

client.connect(Chain.fantom, {
  logo: "Provide Your App Logo URL",
  name: "Your App Name",
  callbackURL: "Your App Uri Schema"
})
```

#### Lite without any handler (Example With React Native)

```javascript
import { Client, Chain } from '@coin98-com/connect-sdk/dist/lite'
import { Linking } from 'react-native'
const client = new Client({
  callback(cUrl){
    Linking.openURL(cUrl);
  }
})

client.connect(Chain.fantom, {
  logo: "Dapps Logo URL",
  name: "Dapps Name",
  callbackURL: "Application URI Schema"
})
```



#### Common API

```ts
// Common API
client.request({
  method: "<Your Request Method Here>",
  params: [],
  redirect: "(Optional), Callback URL after handle request"
}): Promise<{ result, error }>
```

#### Available Methods

**EVM**

Example

\---- SEND ETH ----

```
const onEthSendTransaction = async () => {
  try {
    const result = await window.coin98.provider.request({
      method: 'eth_sendTransaction',
      params: [
        {
          from: <Your Account>,
          to: <Your Account>,
          value: '0x0'
        }
      ]
    })
    return result
  } catch (err) {
    console.log({ err })
  }
}
```

\---- ETH SIGN ----

```
const onEthSign = async () => {
  try {
    const msg = 'Coin98 Connect Example Message'

    const result = await window.coin98.provider.request({
      method: 'eth_sign',
      params: [msg]
    })
    return result
  } catch (err) {
    console.log({ err })
  }
}

```

Document reference

{% embed url="https://docs.coin98.com/developer-guide/ethereum-dapps-integration" %}

**Solana**

Example

\---- SIGN TRANSACTION ----

```
const solSignTransaction = async () => {
  try {
    const pubKey = new PublicKey(solanaAccount)
    const txs = new Transaction().add(SystemProgram.transfer({
      fromPubkey: pubKey,
      toPubkey: pubKey,
      lamports: LAMPORTS_PER_SOL / 100
    }))

    txs.recentBlockhash = (await cnn.getLatestBlockhash()).blockhash
    txs.feePayer = pubKey

    const result = await window.coin98.sol.request({
      method: 'sol_sign',
      params: [txs]
    })

    return result
  } catch (err) {
    console.log({ err })
  }
}
```

\---- SIGN MESSAGES ----

```
const solSignMessage = async () => {
  try {
    const result = await window.coin98.sol.request({
      method: 'sol_signMessage',
      params: ['Some Message Should Goes Here']
    })
    return result
  } catch (err) {
    console.log({ err })
  }
}
```

Document reference

{% embed url="https://docs.coin98.com/developer-guide/solana-dapps-integration" %}

**NEAR**

Example

\---- SIGN & SEND TRANSACTIONS ----

```
const nearSignAndSend = async () => {
  try {
    const result = await window.coin98.near.request({
      method: 'near_signAndSendTransaction',
      params: [{
        transactions: [{
          action: 'transfer',
          amount: 0.001
        }],
        receiver: resultAcc.accountId
      }]
    })
    return get(result, 'result') || result
  } catch (err) {
    console.log({ err })
  }
}
```

\---- SIGN NEAR ACCOUNT STATE ----

```
const getNearAccountState = async () => {
  try {
    const result = await window.coin98.near.request({
      method: 'near_accountState',
    })
    const ressultAccState = get(result, 'result') || result
    return ressultAccState
  } catch (err) {
    console.log({ err })
  }
}
```







Document reference

{% embed url="https://docs.coin98.com/developer-guide/near-dapps-integration" %}

**Cosmos**

**updating ...**
