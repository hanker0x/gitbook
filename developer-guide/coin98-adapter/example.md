# Example

### Switch Network

```javascript
import { CHAINS_ID } from '@coin98/wallet-adapter-react';

(async () => {
    const result = await switchNetwork(CHAINS_ID.binanceSmartTest, async (error) => {
          {
            const wallet = window.ethereum;
    
            return await wallet.request({
              method: 'wallet_addEthereumChain',
              params: [
                {
                  chainId: CHAINS_ID.binanceSmartTest,
                  chainName: 'Binance Smart Chain Testnet',
    
                  nativeCurrency: {
                    name: 'BNB',
                    symbol: 'BNB', // 2-6 characters long
                    decimals: 18,
                  },
                  rpcUrls: ['https://data-seed-prebsc-1-s1.binance.org:8545/'],
                },
              ],
            });
          }
        });    
  };();
```

### Send Transaction

```javascript
(async () => {
    const transaction = {
      to: /* contract address */,
      from: /* from address */,
      value: /* value */,
      data: /* contract data */,
      chainId: /* chainId */,
    };

    const result = await sendTransaction(transaction);
  };)();
```

### Sign Message

```javascript
(async () => {
    const result = await signMessage('Coin98 Adapter');
  };();
```

### Sign Typed Data V1

```javascript
const { selectedChainId } = useWallet();

(async () => {
    const msgParams = [
          {
            type: 'string',
            name: 'Message',
            value: 'Hi, Alice!',
          },
          {
            type: 'uint32',
            name: 'A number',
            value: '1337',
          },
        ];
    const result = await signTypedData(msgParams, 'v1');
  };();
```

### Sign Typed Data V3

```javascript
const { selectedChainId } = useWallet();

(async () => {
    const msgParams = {
      types: {
        EIP712Domain: [
          { name: 'name', type: 'string' },
          { name: 'version', type: 'string' },
          { name: 'chainId', type: 'uint256' },
          { name: 'verifyingContract', type: 'address' },
        ],
        Person: [
          { name: 'name', type: 'string' },
          { name: 'wallet', type: 'address' },
        ],
        Mail: [
          { name: 'from', type: 'Person' },
          { name: 'to', type: 'Person' },
          { name: 'contents', type: 'string' },
        ],
      },
      primaryType: 'Mail',
      domain: {
        name: 'Ether Mail',
        version: '1',
        chainId: parseInt(selectedChainId),
        verifyingContract: '0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC',
      },
      message: {
        from: {
          name: 'Cow',
          wallet: '0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826',
        },
        to: {
          name: 'Bob',
          wallet: '0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB',
        },
        contents: 'Hello, Bob!',
      },
    };
    const result = await signTypedData(msgParams, 'v3');
  };();
```

### Sign Typed Data V4

```javascript
const { selectedChainId } = useWallet();

(async () => {
    const msgParams = {
          domain: {
            chainId: parseInt(selectedChainId),
            name: 'Ether Mail',
            verifyingContract: '0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC',
            version: '1',
          },
          message: {
            contents: 'Hello, Bob!',
            from: {
              name: 'Cow',
              wallets: ['0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826', '0xDeaDbeefdEAdbeefdEadbEEFdeadbeEFdEaDbeeF'],
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
        };
    const result = await signTypedData(msgParams, 'v4');
  };();
```

### ETH Sign

```javascript
(async () => {
    const result = await ethSign(message);
  };();
```

### Watch Asset

<pre class="language-javascript"><code class="lang-javascript">(async () => {
    const params = {
      type: 'ERC20',
      options: {
        address: '0x5542b596F198d8952B33DFEf3498eDC1f2D6AA42',
        symbol: 'CHIPO',
        decimals: 18,
        image: 'https://metamask.github.io/test-dapp/metamask-fox.svg',
      },
    };
<strong>    const result = await watchAsset(params);
</strong>  };();
</code></pre>

### Get Encryption Public Key

```javascript
(async () => {
    const result = await getEncryptionPublicKey();
  };();
```

### ETH Decrypt

```javascript
(async () => {
    const res = await ethDecrypt(encryptedMessage);
  };();
```
