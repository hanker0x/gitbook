# Sui DApps Integration

(Require Extension version 7.2.4++)

Since version 7.2.4 Coin98 Wallet Extension support Wallet Standard, it's automatically integrated with [Sui Wallet Kit](https://sui-wallet-kit.vercel.app/advanced/wallet-standard) & [Suiet Wallet Kit](https://kit.suiet.app/) If you want manual integration, please read carefully guide below:

## Manual Integration

Coin98 Wallet exposes Sui Wallet Interface at `window.coin98.sui`

### API Reference:

#### Connect

```typescript
const accounts: string[] = await window.coin98.sui.connect();
// Handle your transformation from here for list of address;
```

#### Sign Transaction Block

```typescript
import type { SignedTransaction, TransactionBlock, SuiTransactionBlockResponse } from "@mysten/sui.js";

interface SuiSignTransactionBlockInput {
    transactionBlock: TransactionBlock;
}

const txBlockInput: SuiSignTransactionBlockInput = { 
	transactionBlock: { 
		// Your tx block
	} 
}
const signed: SignedTransaction  = window.coin98.sui.signTransactionBlock(txBlockInput)

// If you want to sign and broadcast, try the following api
const executeResult: SuiTransactionBlockResponse = await window.coin98.sui.signAndExecuteTransactionBlock(txBlockInput)
```

### Sign Message

```typescript
import { SignedMessage } from "@mysten/sui.js";
interface SuiSignMessageInput {
    message: Uint8Array;
}

const input: SuiSignMessageInput  =  { 
	message: <Your Uint8Array message>
}

const signedMessage: SignedMessage = window.coin98.sui.signMessage(input);
```
