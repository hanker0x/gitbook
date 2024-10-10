# Setup Coin98 Adapter for React

### Setup for React

```javascript
// Configure blockchains, wallets then wrap app with WalletProvider, WalletModalProvider
import {
  BLOCKCHAINS_DATA,
  WalletProvider,
} from "@coin98-com/wallet-adapter-react";
import { WalletModalProvider } from "@coin98-com/wallet-adapter-react-ui";
import { Coin98WalletAdapter } from "@coin98-com/wallet-adapter-coin98";
import { MetaMaskWalletAdapter } from "@coin98-com/wallet-adapter-metamask";

const Coin98AdapterProvider = ({ children }) => {
  const enables = [BLOCKCHAINS_DATA.ethereum];
  const wallets = [Coin98WalletAdapter, MetaMaskWalletAdapter];
  return (
    <WalletProvider wallets={wallets} enables={enables} autoConnect>
      <WalletModalProvider>{children}</WalletModalProvider>
    </WalletProvider>
  );
};

export default Coin98AdapterProvider;
```

```javascript
// modal.js
import { evmChains, WalletModalC98 } from "@coin98-com/wallet-adapter-react-ui";

const Coin98AdapterModal = () => {
  const defaultChains = [...evmChains]; // multi-chain
  // const defaultChains = [tomo,ether]; // single-chain
  return <WalletModalC98 isC98Theme enableChains={defaultChains} />;
};

export default Coin98AdapterModal;
```

Common props:

```typescript
className?: string;
container?: string;
enableChains?: ChainInfo[];
activeChainId?: string;
titleModal?: string | ReactNode;
titleWallets?: string | ReactNode;›
titleNetworks?: string | ReactNode;
layoutClass?: string;
layoutStyle?: CSSProperties;
overlayStyle?: CSSProperties;
overlayClass?: string;
isC98Theme?: boolean;
renderListChains?: (chainData: ChainInfo, isActive: boolean) => ReactNode;
renderListWallets?: (walletIcon: string, walletName: string) => ReactNode;
```

```javascript
// index.js
...
import Coin98AdapterProvider from "./provider";
import Coin98AdapterModal from "./modal";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <Coin98AdapterProvider>
      <App />
      <Coin98AdapterModal />
    </Coin98AdapterProvider>
  </React.StrictMode>
);
```

Then use it in your dApp

```javascript
// app.js
import { useWallet } from '@coin98-com/wallet-adapter-react';
import { useWalletModal } from '@coin98-com/wallet-adapter-react-ui';

import '@coin98-com/wallet-adapter-react-ui/styles.css';

export default function App() {
  const { address, selectedChainId, disconnect, selectedBlockChain, connected } = useWallet();
  const { openWalletModal } = useWalletModal();

  return (
    <>
      <div>
        {!connected && <button onClick={openWalletModal}>Connect</button>}
        {connected && <button onClick={disconnect}>Disconnect</button>}
      </div>

      <div>
        {connected && (
          <div>
            <div>
              Address: <span>{address}</span>
            </div>

            <div>
              Network: <span>{selectedChainId}</span>
            </div>

            <div>
              Blockchain: <span>{selectedBlockChain}</span>
            </div>
          </div>
        )}
      </div>
    </>
  );
}
```
