# {{PROJECT_NAME}} - Ethers.js Blockchain Integration

Complete blockchain integration templates using Ethers.js 6.x, Foundry, wagmi, and React.

**Author:** {{AUTHOR}}

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Web3.js vs Ethers.js Comparison](#web3js-vs-ethersjs-comparison)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Smart Contracts](#smart-contracts)
- [Services](#services)
- [React Integration](#react-integration)
- [Testing](#testing)
- [Deployment](#deployment)
- [Best Practices](#best-practices)
- [Resources](#resources)

---

## Overview

This template provides a complete blockchain integration setup with:

- **Ethers.js 6.x** - Modern Ethereum library
- **Foundry** - Fast Solidity framework for contracts
- **wagmi** - React hooks for Ethereum
- **TypeScript** - Full type safety
- **Production-ready contracts** - ERC-20, ERC-721, Staking, Governance

---

## Features

### Smart Contracts (Solidity)

- ✅ **ERC-20 Token** with permit, votes, mint/burn
- ✅ **ERC-721 NFT** with enumerable, URI storage
- ✅ **Staking Contract** with rewards and lock periods
- ✅ **Governance (DAO)** with timelock control
- ✅ **Foundry tests** with 100% coverage
- ✅ **Gas optimized** and audited patterns

### TypeScript/JavaScript

- ✅ **Service layer** for each contract type
- ✅ **React hooks** for wallet, tokens, NFTs, staking
- ✅ **React components** ready to use
- ✅ **Multicall** for batched reads
- ✅ **EIP-712** typed data signing
- ✅ **ENS** resolution
- ✅ **Type-safe** with TypeScript strict mode

### Developer Experience

- ✅ **Hot reload** with tsx
- ✅ **Testing** with Vitest
- ✅ **Type generation** with TypeChain
- ✅ **Multi-chain** support (Mainnet, Sepolia, Polygon, Arbitrum, Optimism, Base)
- ✅ **RainbowKit** integration for wallet UI
- ✅ **Comprehensive examples**

---

## Web3.js vs Ethers.js Comparison

### Why Ethers.js?

| Feature | Web3.js | Ethers.js |
|---------|---------|-----------|
| **Bundle Size** | ~440 KB | ~116 KB |
| **TypeScript** | Basic types | Full type safety |
| **Modern Syntax** | Callback-based | Promise/async-await |
| **ENS Support** | Manual | Built-in |
| **Human-Readable ABI** | No | Yes |
| **Wallet Support** | Limited | Extensive |
| **Documentation** | Good | Excellent |
| **Active Development** | Moderate | Very active |
| **Community** | Large | Growing fast |
| **React Integration** | Manual | wagmi library |

### Code Comparison

#### Connecting to Provider

**Web3.js:**
```javascript
const Web3 = require('web3');
const web3 = new Web3('https://mainnet.infura.io/v3/YOUR-PROJECT-ID');
```

**Ethers.js:**
```typescript
import { JsonRpcProvider } from 'ethers';
const provider = new JsonRpcProvider('https://mainnet.infura.io/v3/YOUR-PROJECT-ID');
```

#### Getting Balance

**Web3.js:**
```javascript
web3.eth.getBalance(address, (err, balance) => {
  if (!err) {
    console.log(web3.utils.fromWei(balance, 'ether'));
  }
});
```

**Ethers.js:**
```typescript
const balance = await provider.getBalance(address);
console.log(ethers.formatEther(balance));
```

#### Contract Interaction

**Web3.js:**
```javascript
const contract = new web3.eth.Contract(ABI, address);
const balance = await contract.methods.balanceOf(holder).call();
await contract.methods.transfer(to, amount).send({ from: sender });
```

**Ethers.js:**
```typescript
const contract = new Contract(address, ABI, provider);
const balance = await contract.balanceOf(holder);
const tx = await contract.connect(signer).transfer(to, amount);
await tx.wait();
```

#### Human-Readable ABI

**Web3.js:**
```javascript
// Must use full JSON ABI
const ABI = [{"constant":true,"inputs":[{"name":"_owner","type":"address"}],...}];
```

**Ethers.js:**
```typescript
// Can use human-readable format
const ABI = [
  'function balanceOf(address owner) view returns (uint256)',
  'function transfer(address to, uint256 amount) returns (bool)'
];
```

### When to Use Each

**Use Ethers.js when:**
- Building new projects
- Need TypeScript support
- Want smaller bundle size
- Using React (with wagmi)
- Need ENS support
- Want modern async/await syntax

**Use Web3.js when:**
- Maintaining legacy code
- Need specific Web3.js plugins
- Team is already familiar with it
- Using older Node.js versions

**Verdict:** Ethers.js is the modern choice for most projects.

---

## Project Structure

```
blockchain/ethersjs/
├── contracts/               # Solidity contracts (Foundry)
│   ├── src/
│   │   ├── Token.sol       # ERC-20 token
│   │   ├── NFT.sol         # ERC-721 NFT
│   │   ├── Staking.sol     # Staking contract
│   │   └── Governance.sol  # DAO governance
│   ├── script/
│   │   └── Deploy.s.sol    # Deployment script
│   └── test/
│       └── Token.t.sol     # Contract tests
├── src/
│   ├── config/             # Chain and ABI configs
│   ├── providers/          # Ethers.js providers
│   ├── services/           # Contract service classes
│   ├── hooks/              # React hooks
│   ├── components/         # React components
│   ├── utils/              # Utilities
│   └── types/              # TypeScript types
├── scripts/                # Deployment scripts
├── test/                   # Integration tests
└── package.json
```

---

## Getting Started

### 1. Installation

```bash
# Install dependencies
npm install

# Or with yarn
yarn install
```

### 2. Environment Setup

Copy `.env.example` to `.env`:

```bash
cp .env.example .env
```

Update with your values:

```env
# RPC URLs (get from Alchemy, Infura, etc.)
MAINNET_RPC_URL=https://eth-mainnet.g.alchemy.com/v2/YOUR_API_KEY
SEPOLIA_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/YOUR_API_KEY

# Private keys (NEVER commit!)
PRIVATE_KEY=your_private_key_here

# Etherscan API keys (for verification)
ETHERSCAN_API_KEY=your_etherscan_api_key

# WalletConnect
WALLETCONNECT_PROJECT_ID=your_walletconnect_project_id
```

### 3. Install Foundry

```bash
# Install Foundry
curl -L https://foundry.paradigm.xyz | bash
foundryup

# Install OpenZeppelin contracts
forge install OpenZeppelin/openzeppelin-contracts
```

### 4. Compile Contracts

```bash
# Compile with Foundry
npm run foundry:build

# Generate TypeScript types
npm run generate:types
```

---

## Smart Contracts

### Token (ERC-20)

Features:
- ERC-20 standard with permit (EIP-2612)
- Voting capabilities (ERC-20Votes)
- Mintable and burnable
- Max supply cap

```solidity
Token token = new Token(
    "My Token",      // name
    "MTK",           // symbol
    1_000_000e18,    // max supply
    100_000e18       // initial supply
);
```

### NFT (ERC-721)

Features:
- ERC-721 standard with enumerable
- URI storage for metadata
- Public minting with price
- Owner-only custom URI minting

```solidity
NFT nft = new NFT(
    "My NFT",                    // name
    "MNFT",                      // symbol
    "ipfs://QmBaseURI/",        // base URI
    10000,                       // max supply
    0.01 ether                   // mint price
);
```

### Staking

Features:
- Stake tokens to earn rewards
- Configurable reward rate
- Lock period for withdrawals
- Automatic reward calculation

```solidity
Staking staking = new Staking(
    tokenAddress,     // staking token
    rewardToken,      // reward token
    1e18 / 86400,     // 1 token per day
    100e18,           // min stake: 100 tokens
    7 days            // lock period
);
```

### Governance (DAO)

Features:
- OpenZeppelin Governor
- Timelock control
- Configurable voting parameters
- On-chain proposal execution

```solidity
Governance governor = new Governance(
    votingToken,      // voting token (ERC-20Votes)
    timelock,         // timelock controller
    1,                // voting delay (blocks)
    50400,            // voting period (~7 days)
    1000e18,          // proposal threshold
    4                 // quorum (4%)
);
```

---

## Services

### ERC20Service

```typescript
import { createERC20Service } from './services/erc20-service';

const service = createERC20Service(tokenAddress, chainId);

// Get token info
const info = await service.getTokenInfo();

// Get balance
const balance = await service.balanceOf(address);

// Transfer
const tx = await service.transfer(to, amount);
await service.waitForTransaction(tx);
```

### ERC721Service

```typescript
import { createERC721Service } from './services/erc721-service';

const service = createERC721Service(nftAddress, chainId);

// Get NFTs of owner
const nfts = await service.getNFTsOfOwner(address);

// Get NFT with metadata
const nft = await service.getNFT(tokenId);

// Transfer
const tx = await service.safeTransferFrom(from, to, tokenId);
```

### StakingService

```typescript
import { createStakingService } from './services/staking-service';

const service = createStakingService(stakingAddress, chainId);

// Get staking info
const info = await service.getStakingInfo();

// Get user stake
const stake = await service.getStakeInfo(address);

// Stake tokens
const tx = await service.stake(amount);

// Claim rewards
const tx = await service.claimReward();
```

### MulticallService

```typescript
import { createMulticallService } from './services/multicall-service';

const service = createMulticallService(chainId);

// Get multiple token balances
const balances = await service.getTokenBalances(
  [token1, token2, token3],
  holderAddress
);

// Get multiple token info
const tokensInfo = await service.getTokensInfo([token1, token2]);
```

---

## React Integration

### Hooks

#### useEthers

```typescript
import { useEthers } from './hooks/useEthers';

function App() {
  const { account, chainId, balance, connect, disconnect } = useEthers();

  return (
    <div>
      {account ? (
        <button onClick={disconnect}>Disconnect</button>
      ) : (
        <button onClick={connect}>Connect Wallet</button>
      )}
    </div>
  );
}
```

#### useTokenBalance

```typescript
import { useTokenBalance } from './hooks/useTokenBalance';

function TokenDisplay() {
  const { balance, formattedBalance, symbol, isLoading } = useTokenBalance({
    tokenAddress,
    holderAddress: account,
    chainId,
    refreshInterval: 10000, // 10 seconds
  });

  return <div>Balance: {formattedBalance} {symbol}</div>;
}
```

#### useNFT

```typescript
import { useNFT } from './hooks/useNFT';

function NFTGallery() {
  const { nfts, isLoading, refetch } = useNFT({
    nftAddress,
    ownerAddress: account,
    chainId,
  });

  return (
    <div>
      {nfts.map(nft => (
        <div key={nft.tokenId.toString()}>
          <img src={nft.metadata?.image} />
          <h3>{nft.metadata?.name}</h3>
        </div>
      ))}
    </div>
  );
}
```

### Components

Ready-to-use React components:

- **WalletConnect** - Wallet connection button
- **ChainSelector** - Network switcher
- **TokenTransfer** - Token transfer form
- **NFTGallery** - Display user's NFTs
- **StakingPanel** - Stake/unstake interface

```typescript
import { WalletConnect, ChainSelector, TokenTransfer } from './components';

function App() {
  return (
    <div>
      <WalletConnect />
      <ChainSelector />
      <TokenTransfer tokenAddress={TOKEN_ADDRESS} chainId={1} />
    </div>
  );
}
```

---

## Testing

### Unit Tests (Foundry)

```bash
# Run contract tests
npm run foundry:test

# With gas report
forge test --gas-report

# Specific test
forge test --match-test testTransfer -vvv
```

### Integration Tests (Vitest)

```bash
# Run all tests
npm test

# Watch mode
npm test -- --watch

# With UI
npm run test:ui

# Coverage
npm test -- --coverage
```

---

## Deployment

### Using Foundry Script

```bash
# Deploy to Sepolia
forge script script/Deploy.s.sol:DeployScript \
  --rpc-url $SEPOLIA_RPC_URL \
  --broadcast \
  --verify \
  -vvvv
```

### Using TypeScript Script

```bash
# Set environment
export CHAIN_ID=11155111
export PRIVATE_KEY=your_key

# Deploy
npm run deploy
```

### Verify Contracts

```bash
# Verify on Etherscan
forge verify-contract \
  --chain-id 11155111 \
  --constructor-args $(cast abi-encode "constructor(string,string,uint256,uint256)" "Token" "TKN" 1000000 100000) \
  <CONTRACT_ADDRESS> \
  src/Token.sol:Token
```

---

## Best Practices

### Security

1. **Never commit private keys** - Use `.env` and `.gitignore`
2. **Verify contracts** - Always verify on Etherscan
3. **Audit before mainnet** - Get professional audit
4. **Use multisig** - For ownership and admin functions
5. **Test extensively** - 100% coverage minimum
6. **Check approvals** - Validate user approvals before transfers

### Gas Optimization

1. **Use multicall** - Batch read operations
2. **Cache storage** - Store frequently accessed values
3. **Optimize loops** - Minimize iterations
4. **Pack variables** - Use struct packing
5. **Use events** - For off-chain data
6. **Estimate gas** - Before sending transactions

### Code Quality

1. **TypeScript strict mode** - Enable all checks
2. **ESLint** - Follow style guide
3. **Prettier** - Format code
4. **Type generation** - Use TypeChain
5. **Error handling** - Always catch and handle errors
6. **Documentation** - Document all functions

### Performance

1. **Provider caching** - Reuse provider instances
2. **Lazy loading** - Load contracts on demand
3. **Debounce** - For frequent updates
4. **Pagination** - For large lists
5. **Indexing** - Use The Graph for queries
6. **WebSockets** - For real-time updates

---

## Resources

### Documentation

- [Ethers.js Docs](https://docs.ethers.org/v6/)
- [Foundry Book](https://book.getfoundry.sh/)
- [wagmi Docs](https://wagmi.sh/)
- [OpenZeppelin Contracts](https://docs.openzeppelin.com/contracts/)
- [Solidity Docs](https://docs.soliditylang.org/)

### Tools

- [Alchemy](https://www.alchemy.com/) - RPC provider
- [Tenderly](https://tenderly.co/) - Debugging & monitoring
- [The Graph](https://thegraph.com/) - Indexing
- [Hardhat](https://hardhat.org/) - Alternative to Foundry

### Community

- [Ethereum Stack Exchange](https://ethereum.stackexchange.com/)
- [ethers.js Discord](https://discord.gg/ethers)
- [Foundry Telegram](https://t.me/foundry_rs)

---

## License

MIT

## Author

{{AUTHOR}}

---

**Ready to build on Ethereum with Ethers.js!** 🚀
