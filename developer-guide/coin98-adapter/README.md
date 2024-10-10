# Coin98 Adapter

## Introduction

**Coin98 Adapter** is built to make the integration between your Multi-chain Dapp and Wallet easier than ever. Powered by Coin98 Multi-chain core engine, Coin98 Adapter can handle the connection and functions for most of the popular Blockchains and Wallets on the market.

**Coin98 Adapter** provides an easy connect modal UI with Light/Dark mode available to choose. For more advanced use cases, you can use your owned dapp connection UI and call the supported Adapter functions instead.&#x20;

<figure><img src="../../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

## Supported Chains & Wallets

Coin98 Adapter currently supports most of the popular EVM Chains, more Chains and Wallets will be supported in the future.

| Blockchain             | Coin98 Wallet (Super Wallet & Extension) | Metamask (Extension) |
| ---------------------- | ---------------------------------------- | -------------------- |
| Arbitrum               | Supported                                | Supported            |
| Aurora                 | Supported                                | Supported            |
| Avalanche C-Chain      | Supported                                | Supported            |
| Base                   | Supported                                | Supported            |
| Base Testnet           | Supported                                | Supported            |
| Bitkub Chain           | Supported                                | Supported            |
| BitTorrent Chain       | Supported                                | Supported            |
| BNB Smart Chain        | Supported                                | Supported            |
| Boba Network           | Supported                                | Supported            |
| Celo                   | Supported                                | Supported            |
| Cronos                 | Supported                                | Supported            |
| Ethereum               | Supported                                | Supported            |
| Ethereum PoW           | Supported                                | Supported            |
| Fantom                 | Supported                                | Supported            |
| GateChain              | Supported                                | Supported            |
| Gnonsis Chain          | Supported                                | Supported            |
| Harmony                | Supported                                | Supported            |
| HECO Chain             | Supported                                | Supported            |
| KardiaChain            | Supported                                | Supported            |
| Klaytn                 | Supported                                | Supported            |
| KuCoin Community Chain | Supported                                | Supported            |
| Moonbeam               | Supported                                | Supported            |
| Oasis Network          | Supported                                | Supported            |
| OKX Chain              | Supported                                | Supported            |
| Optimism               | Supported                                | Supported            |
| PlatON Network         | Supported                                | Supported            |
| Polygon                | Supported                                | Supported            |
| Polygon zkEVM          | Supported                                | Supported            |
| Ronin                  | Supported                                | Supported            |
| Theta Network          | Supported                                | Supported            |
| Viction                | Supported                                | Supported            |
| zkSync Era             | Supported                                | Supported            |

## Install

To use the Coin98 Adapter, first install the core package from the NPM registry.

```typescript
npm install \
@coin98-com/wallet-adapter-react \
@coin98-com/wallet-adapter-react-ui
```

Then, install the wallets you want to support. More wallets will be supported in the future.

```typescript
npm install \
@coin98-com/wallet-adapter-coin98 \
@coin98-com/wallet-adapter-metamask
```

## Setup Coin98 Adapter

### Setup for Next.js

Follow this [link](setup-coin98-adapter-for-next.js.md) for more details.

### Setup for React

Follow this [link](setup-coin98-adapter-for-react.md) for more details.
