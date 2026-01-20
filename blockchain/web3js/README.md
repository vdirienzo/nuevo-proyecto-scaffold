# {{PROJECT_NAME}} - Web3.js Blockchain Integration

Complete Web3.js blockchain integration with smart contracts, services, React hooks, and UI components.

**Author:** {{AUTHOR}}

## Features

- ✅ **Web3.js 4.x** - Latest version with full TypeScript support
- ✅ **Smart Contracts** - ERC-20 Token, ERC-721 NFT, NFT Marketplace
- ✅ **Multi-chain Support** - Ethereum, Polygon, BSC
- ✅ **Wallet Integration** - MetaMask, WalletConnect
- ✅ **React Hooks** - Custom hooks for Web3 operations
- ✅ **UI Components** - Ready-to-use React components
- ✅ **Gas Estimation** - Automatic gas price optimization
- ✅ **Transaction Tracking** - Real-time transaction monitoring
- ✅ **Event Listening** - Subscribe to contract events
- ✅ **Contract Verification** - Etherscan/Polygonscan integration
- ✅ **Testing** - Comprehensive test suite with Hardhat
- ✅ **TypeScript** - Full type safety

## Table of Contents

- [Installation](#installation)
- [Configuration](#configuration)
- [Smart Contracts](#smart-contracts)
- [Deployment](#deployment)
- [Usage](#usage)
  - [Services](#services)
  - [React Hooks](#react-hooks)
  - [React Components](#react-components)
- [Testing](#testing)
- [Scripts](#scripts)
- [Directory Structure](#directory-structure)
- [Examples](#examples)
- [License](#license)

## Installation

```bash
# Install dependencies
npm install

# or
yarn install
```

## Configuration

### 1. Environment Variables

Copy `.env.example` to `.env`:

```bash
cp .env.example .env
```

Configure your environment variables:

```env
# Private key (NEVER commit this!)
PRIVATE_KEY=your_private_key_here

# RPC URLs (get from Alchemy, Infura, etc.)
MAINNET_RPC_URL=https://eth-mainnet.g.alchemy.com/v2/your_api_key
SEPOLIA_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/your_api_key
POLYGON_RPC_URL=https://polygon-mainnet.g.alchemy.com/v2/your_api_key

# API Keys for contract verification
ETHERSCAN_API_KEY=your_etherscan_api_key
POLYGONSCAN_API_KEY=your_polygonscan_api_key
BSCSCAN_API_KEY=your_bscscan_api_key

# WalletConnect
WALLETCONNECT_PROJECT_ID=your_walletconnect_project_id
```

### 2. Update Contract Addresses

After deployment, update contract addresses in `src/config/contracts.ts`:

```typescript
export const CONTRACT_ADDRESSES: Record<number, ContractAddresses> = {
  1: { // Ethereum Mainnet
    Token: "0x...",
    NFT: "0x...",
    Marketplace: "0x...",
  },
  // ... other networks
};
```

## Smart Contracts

### Token (ERC-20)

Standard ERC-20 token with additional features:
- Mintable (owner only)
- Burnable
- Pausable
- Max supply: 1 billion tokens

### NFT (ERC-721)

NFT collection with:
- Public minting (paid)
- Owner minting (free)
- Batch minting
- Configurable max supply and mint price
- Metadata URI storage

### Marketplace

NFT marketplace with:
- List NFTs for sale
- Buy NFTs (ETH or ERC-20 tokens)
- Make offers
- Accept/cancel offers
- Configurable marketplace fee

## Deployment

### Local Development

1. Start local Hardhat node:

```bash
npm run node
```

2. Deploy contracts:

```bash
npm run deploy:localhost
```

### Testnet Deployment

Deploy to Sepolia testnet:

```bash
npm run deploy:sepolia
```

Deploy to other testnets:

```bash
npm run deploy:polygon  # Polygon Mumbai
npm run deploy:bsc      # BSC Testnet
```

### Verify Contracts

After deployment, verify on block explorers:

```bash
npm run verify:sepolia
npm run verify:polygon
npm run verify:bsc
```

## Usage

### Services

#### Web3 Provider

```typescript
import { web3Provider } from "./services/web3-provider";

// Initialize with MetaMask
await web3Provider.initializeWithMetaMask();

// Get Web3 instance
const web3 = web3Provider.getWeb3();

// Switch network
await web3Provider.switchNetwork(137); // Polygon
```

#### Wallet Service

```typescript
import { walletService } from "./services/wallet-service";

// Connect wallet
const account = await walletService.connectMetaMask();

// Get balance
const balance = await walletService.getBalance(account);

// Sign message
const signature = await walletService.signMessage("Hello Web3", account);
```

#### Token Service

```typescript
import { tokenService } from "./services/token-service";

// Initialize
await tokenService.initialize();

// Get token info
const info = await tokenService.getTokenInfo();

// Transfer tokens
const txHash = await tokenService.transfer(from, to, amount);

// Get balance
const balance = await tokenService.getBalance(address);
```

#### NFT Service

```typescript
import { nftService } from "./services/nft-service";

// Initialize
await nftService.initialize();

// Mint NFT
const txHash = await nftService.mint(from, to, "ipfs://...");

// Get NFT metadata
const metadata = await nftService.getNFTMetadata(tokenId);

// Get all NFTs owned by address
const nfts = await nftService.getNFTsOwnedBy(address);
```

#### Transaction Service

```typescript
import { transactionService } from "./services/transaction-service";

// Get transaction details
const details = await transactionService.getTransactionDetails(txHash);

// Wait for confirmation
await transactionService.waitForTransaction(txHash, 12);

// Estimate gas
const estimation = await transactionService.estimateGas(transaction);
```

### React Hooks

#### useWeb3

```typescript
import { useWeb3 } from "./hooks/useWeb3";

function MyComponent() {
  const { web3, chainId, networkName, initialize, switchNetwork } = useWeb3();

  const handleConnect = async () => {
    await initialize();
  };

  return (
    <div>
      <button onClick={handleConnect}>Connect</button>
      <p>Network: {networkName}</p>
    </div>
  );
}
```

#### useWallet

```typescript
import { useWallet } from "./hooks/useWallet";

function WalletInfo() {
  const { account, balance, isConnected, connect, disconnect } = useWallet();

  return (
    <div>
      {isConnected ? (
        <>
          <p>Account: {account}</p>
          <p>Balance: {balance} ETH</p>
          <button onClick={disconnect}>Disconnect</button>
        </>
      ) : (
        <button onClick={() => connect("metamask")}>Connect</button>
      )}
    </div>
  );
}
```

#### useContract

```typescript
import { useContract } from "./hooks/useContract";

function TokenBalance() {
  const { contract, call } = useContract(tokenAddress, tokenABI);
  const [balance, setBalance] = useState("0");

  useEffect(() => {
    if (contract) {
      call("balanceOf", userAddress).then(setBalance);
    }
  }, [contract, userAddress]);

  return <p>Balance: {balance}</p>;
}
```

#### useTransaction

```typescript
import { useTransaction } from "./hooks/useTransaction";

function TransferButton() {
  const { sendTransaction, isPending, isSuccess } = useTransaction();

  const handleTransfer = async () => {
    await sendTransaction(async () => {
      return await tokenService.transfer(from, to, amount);
    });
  };

  return (
    <button onClick={handleTransfer} disabled={isPending}>
      {isPending ? "Transferring..." : "Transfer"}
    </button>
  );
}
```

### React Components

#### ConnectWallet

```typescript
import { ConnectWallet } from "./components/ConnectWallet";

function App() {
  return (
    <ConnectWallet
      onConnect={(account) => console.log("Connected:", account)}
      onDisconnect={() => console.log("Disconnected")}
      showBalance={true}
    />
  );
}
```

#### NetworkSwitch

```typescript
import { NetworkSwitch } from "./components/NetworkSwitch";

function App() {
  return (
    <NetworkSwitch
      onSwitch={(chainId) => console.log("Switched to:", chainId)}
      showCurrentNetwork={true}
    />
  );
}
```

#### TokenBalance

```typescript
import { TokenBalance } from "./components/TokenBalance";

function App() {
  return (
    <TokenBalance
      showSymbol={true}
      decimals={4}
      refreshInterval={30000}
    />
  );
}
```

#### TransactionStatus

```typescript
import { TransactionStatus } from "./components/TransactionStatus";

function App() {
  const [txHash, setTxHash] = useState(null);

  return (
    <TransactionStatus
      hash={txHash}
      onSuccess={(hash) => console.log("Success:", hash)}
      onError={(error) => console.error("Error:", error)}
      showExplorerLink={true}
    />
  );
}
```

## Testing

Run tests with Hardhat:

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run specific test file
npx hardhat test test/Token.test.ts
```

## Scripts

### Development

```bash
npm run dev              # Watch mode for TypeScript
npm run build            # Build TypeScript
npm run clean            # Clean build artifacts
```

### Contracts

```bash
npm run compile          # Compile contracts
npm run test             # Run tests
npm run test:coverage    # Run tests with coverage
npm run node             # Start local node
```

### Deployment

```bash
npm run deploy:localhost  # Deploy to localhost
npm run deploy:sepolia    # Deploy to Sepolia
npm run deploy:polygon    # Deploy to Polygon
npm run deploy:bsc        # Deploy to BSC
```

### Verification

```bash
npm run verify:sepolia   # Verify on Etherscan
npm run verify:polygon   # Verify on Polygonscan
npm run verify:bsc       # Verify on BscScan
```

### Code Quality

```bash
npm run lint             # Lint TypeScript and Solidity
npm run lint:fix         # Fix lint issues
npm run format           # Format code with Prettier
```

## Directory Structure

```
blockchain/web3js/
├── contracts/              # Solidity smart contracts
│   ├── Token.sol
│   ├── NFT.sol
│   ├── Marketplace.sol
│   └── interfaces/
├── scripts/                # Deployment scripts
│   ├── deploy.ts
│   ├── verify.ts
│   └── interact.ts
├── test/                   # Contract tests
│   ├── Token.test.ts
│   └── NFT.test.ts
├── src/                    # TypeScript source
│   ├── config/            # Configuration
│   ├── services/          # Service layer
│   ├── hooks/             # React hooks
│   ├── components/        # React components
│   ├── utils/             # Utilities
│   └── types/             # Type definitions
├── artifacts/              # Compiled contracts
├── deployments/            # Deployment info
├── .env.example            # Environment template
├── hardhat.config.ts       # Hardhat configuration
├── package.json
├── tsconfig.json
└── README.md
```

## Examples

### Complete Token Transfer Flow

```typescript
import { web3Provider } from "./services/web3-provider";
import { walletService } from "./services/wallet-service";
import { tokenService } from "./services/token-service";

async function transferTokens() {
  // 1. Initialize Web3
  await web3Provider.initializeWithMetaMask();

  // 2. Connect wallet
  const account = await walletService.connectMetaMask();

  // 3. Initialize token service
  await tokenService.initialize();

  // 4. Check balance
  const balance = await tokenService.getBalance(account);
  console.log("Balance:", balance);

  // 5. Transfer tokens
  const recipient = "0x...";
  const amount = "100"; // in token units
  const txHash = await tokenService.transferFormatted(account, recipient, amount);

  console.log("Transaction:", txHash);
}
```

### Complete NFT Minting Flow

```typescript
import { web3Provider } from "./services/web3-provider";
import { walletService } from "./services/wallet-service";
import { nftService } from "./services/nft-service";

async function mintNFT() {
  // 1. Initialize
  await web3Provider.initializeWithMetaMask();
  const account = await walletService.connectMetaMask();
  await nftService.initialize();

  // 2. Get NFT info
  const info = await nftService.getNFTInfo();
  console.log("Mint price:", info.mintPrice);

  // 3. Mint NFT
  const tokenURI = "ipfs://QmExample123";
  const txHash = await nftService.mint(account, account, tokenURI);

  console.log("Minted! Transaction:", txHash);
}
```

## Best Practices

1. **Always validate user input** using utilities from `utils/validation.ts`
2. **Handle errors gracefully** and provide user feedback
3. **Use gas estimation** before sending transactions
4. **Wait for confirmations** for important transactions
5. **Keep private keys secure** - never commit `.env` files
6. **Test thoroughly** on testnets before mainnet deployment
7. **Verify contracts** on block explorers after deployment
8. **Use TypeScript** for type safety
9. **Subscribe to events** for real-time updates
10. **Cache provider** to maintain connection state

## Security Considerations

- ⚠️ Never commit private keys or sensitive data
- ⚠️ Always validate user input
- ⚠️ Use contract pausable functions in emergencies
- ⚠️ Test gas limits to prevent out-of-gas errors
- ⚠️ Verify contracts on block explorers
- ⚠️ Use non-custodial wallets (MetaMask, WalletConnect)
- ⚠️ Implement rate limiting for public endpoints
- ⚠️ Keep dependencies updated

## Troubleshooting

### MetaMask not detected

Ensure MetaMask is installed and enabled in your browser.

### Transaction failing

- Check gas limit and gas price
- Verify contract addresses
- Ensure sufficient balance
- Check network connection

### Contract not verified

- Ensure correct API key in `.env`
- Check that deployment info exists
- Verify constructor arguments match

### Network switch failing

- Ensure network is configured in MetaMask
- Check that chain ID is correct
- Add network manually if needed

## License

MIT

## Author

{{AUTHOR}}

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

## Support

For issues and questions:
- Open an issue on GitHub
- Check existing documentation
- Review example code

---

**Built with ❤️ using Web3.js, Hardhat, and TypeScript**
