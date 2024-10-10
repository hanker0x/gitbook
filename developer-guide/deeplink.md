# Deeplink

A deeplink allows users to interact with Coin98 Super App from supported dapps with available parameters.

For the best experience with a deeplink, it is recommended to create a multichain wallet on Coin98 Super App

URL is embedded in the QR code, hyperlinks to the website, email, or online message (telegram, messenger, instagram, etc.), allowing cross-app communication.

You can use a deeplink for the following activities:&#x20;

Create a URL for users to directly open the DApp and immediately interact with the desired blockchain users will use&#x20;

* Request URL: https://coin98.com/dapp/:link/:chainId&#x20;
* Link: URL Dapps which users will interact with&#x20;
* ChainID: ChainID of the blockchain users will interact with _(if users interact with Solana, Solana chainID will be redirected to the Dapps)_

_Eg. https://coin98.com/dapp/uniswap.com/1_&#x20;

Create URL to let users open the exchange with the desired trading pair.&#x20;

Request URL: https://exchange.coin98.com/:chain/:from/:to

E.g. https://exchange.coin98.com/ether/0xAE12C5930881c53715B369ceC7606B70d8EB229f/0xC285B7E09A4584D027E5BC36571785B515898246

**Supported Chains:**

* Ethereum: ETH
* BNB Chain: BNB&#x20;
* Heco Chain: HT
* Polygon: MATIC
* Avalanche C-Chain: AVAX
* KuCoin Community Chain: KCS
* Fantom: FTM
* Boba Network: BOBA
