# Setup Coin98 Adapter for Next.js

### Setup for Next.js 13

```typescript
// Configure blockchains, wallets then wrap app with WalletProvider, WalletModalProvider
"use client";

import {
  BLOCKCHAINS_DATA,
  WalletProvider,
} from "@coin98-com/wallet-adapter-react";
import { WalletModalProvider } from "@coin98-com/wallet-adapter-react-ui";
import { Coin98WalletAdapter } from "@coin98-com/wallet-adapter-coin98";
import { MetaMaskWalletAdapter } from "@coin98-com/wallet-adapter-metamask";

interface ContainerProps {
  children: React.ReactNode;
}

const Coin98AdapterProvider: React.FC<ContainerProps> = ({ children }) => {
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

```typescript
// modal.tsx
"use client";

import dynamic from "next/dynamic";
import { ChainInfo, evmChains } from "@coin98-com/wallet-adapter-react-ui";

const WalletModalC98 = dynamic(
  async () => (await import("@coin98-com/wallet-adapter-react-ui")).WalletModalC98,
  {
    ssr: false,
  }
);

const Coin98AdapterModal = () => {
  const defaultChains = [...evmChains]; // multi-chain
  // const defaultChains = [tomo,ether]; // single-chainds
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
titleWallets?: string | ReactNode;
titleNetworks?: string | ReactNode;
layoutClass?: string;
layoutStyle?: CSSProperties;
overlayStyle?: CSSProperties;
overlayClass?: string;
isC98Theme?: boolean;
renderListChains?: (chainData: ChainInfo, isActive: boolean) => ReactNode;
renderListWallets?: (walletIcon: string, walletName: string) => ReactNode;
```

Import modal UI CSS

```css
// globals.css
@import '@coin98-com/wallet-adapter-react-ui/styles.css';
```

```typescript
// layout.tsx
...

import "./globals.css";
import Coin98AdapterModal from "./modal";
import Coin98AdapterProvider from "./provider";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    ...
        <Coin98AdapterProvider>
          {children}
          <Coin98AdapterModal />
        </Coin98AdapterProvider>
    ...  
  );
}
```

Then use it in your dApp

```typescript
// page.tsx
"use client";

import { useWallet } from "@coin98-com/wallet-adapter-react";
import { useWalletModal } from "@coin98-com/wallet-adapter-react-ui";

export default function Home() {
  const {
    address,
    selectedChainId,
    disconnect,
    selectedBlockChain,
    connected,
  } = useWallet();
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
