# Capsule ID Vault - Encrypted Health Metrics Analysis

![Capsule ID Vault Logo](./frontend/public/logo.svg)

A privacy-first health metrics analysis DApp using **Fully Homomorphic Encryption (FHE)** technology powered by Zama's FHEVM. Users can submit their health data (BMI, blood sugar, heart rate) in encrypted form, and the smart contract calculates a health score without ever seeing the unencrypted data.

## 🌐 Network Support

This DApp supports **Fully Homomorphic Encryption (FHE)** on the following networks:

- ✅ **Localhost (Chain ID: 31337)** - Using local Hardhat network with `fhevm-hardhat-plugin`
- ✅ **Sepolia Testnet (Chain ID: 11155111)** - Using Zama's FHEVM Sepolia configuration

The contract uses `SepoliaConfig` from `@fhevm/solidity` to enable FHE operations on both networks.

## 🌟 Features

- **Complete Privacy**: Health data is encrypted before submission and remains encrypted on-chain
- **Homomorphic Computation**: Health scores are calculated on encrypted data
- **User-Controlled Decryption**: Only the data owner can decrypt their health metrics
- **Modern UI**: Beautiful, responsive interface built with React and Tailwind CSS
- **Rainbow Wallet Integration**: Seamless wallet connection experience
- **Network Detection**: Automatic detection and warning if connected to wrong network

## 🔐 Privacy Technology

This DApp uses **Fully Homomorphic Encryption (FHE)** which allows:
- Performing calculations on encrypted data
- Zero-knowledge of actual health values by the contract
- Complete privacy while maintaining verifiability

### Health Score Formula

```
Health Score = 3 × BMI + 5 × Blood Sugar + 2 × Heart Rate
```

This weighted sum formula emphasizes:
- **Blood Sugar** (weight: 5) - Highest impact
- **BMI** (weight: 3) - Medium impact  
- **Heart Rate** (weight: 2) - Lower impact

Implemented as:
```solidity
score = 3 * bmi + 5 * bloodSugar + 2 * heartRate
```

**Note:** Division is omitted to avoid complex FHE division operations.

## 📋 Prerequisites

- Node.js v18+ and npm
- Hardhat
- MetaMask or another Web3 wallet
- Git

## 🚀 Quick Start

### 1. Clone the Repository

```bash
cd capsule-id-vault
```

### 2. Install Dependencies

```bash
# Install backend dependencies
npm install

# Install frontend dependencies
cd frontend
npm install
cd ..
```

### 3. Start Local Hardhat Node

```bash
npm run node
```

Keep this terminal running.

### 4. Deploy Contract (New Terminal)

```bash
npm run deploy:local
```

Copy the deployed contract address and update `frontend/src/config/contract.ts`:

```typescript
export const CONTRACT_ADDRESS = '0xYourDeployedContractAddress';
```

### 5. Run Tests

```bash
npm test
```

### 6. Start Frontend

```bash
npm run frontend:dev
```

Visit `http://localhost:3000` in your browser.

## 🧪 Testing

### Local Tests

```bash
npm test
```

This runs the comprehensive test suite on the local Hardhat network.

### Task Commands

```bash
# Check contract address
npx hardhat --network localhost task:address

# Submit health data
npx hardhat --network localhost task:submit-health --bmi 25 --bloodsugar 100 --heartrate 75

# Decrypt and view health data
npx hardhat --network localhost task:decrypt-health

# Get contract statistics
npx hardhat --network localhost task:get-stats

# Check if account has data
npx hardhat --network localhost task:check-data
```

## 📁 Project Structure

```
capsule-id-vault/
├── contracts/              # Solidity smart contracts
│   └── HealthMetrics.sol  # Main FHE health metrics contract
├── deploy/                # Deployment scripts
│   └── deploy.ts
├── test/                  # Test suites
│   ├── HealthMetrics.ts          # Local tests
│   └── HealthMetricsSepolia.ts   # Sepolia tests
├── tasks/                 # Hardhat task scripts
│   ├── accounts.ts
│   └── HealthMetrics.ts
├── frontend/              # React frontend application
│   ├── src/
│   │   ├── components/    # React components
│   │   ├── config/        # Configuration files
│   │   ├── App.tsx        # Main app component
│   │   └── main.tsx       # Entry point
│   └── public/            # Static assets
├── hardhat.config.ts      # Hardhat configuration
├── package.json          # Backend dependencies
└── README.md             # This file
```

## 🔧 Configuration

### Hardhat Configuration

Edit `hardhat.config.ts` to configure networks:

```typescript
networks: {
  localhost: {
    chainId: 31337,
    url: "http://localhost:8545",
  },
  sepolia: {
    chainId: 11155111,
    url: `https://sepolia.infura.io/v3/${INFURA_API_KEY}`,
  },
}
```

### Frontend Configuration

Edit `frontend/src/config/contract.ts`:

```typescript
export const CONTRACT_ADDRESS = 'YOUR_CONTRACT_ADDRESS';
export const NETWORK_CONFIG = {
  localhost: {
    chainId: 31337,
    name: 'Localhost',
    rpcUrl: 'http://localhost:8545',
  },
};
```

### Rainbow Wallet Configuration

Edit `frontend/src/config/wagmi.ts` and add your WalletConnect project ID:

```typescript
projectId: 'YOUR_WALLETCONNECT_PROJECT_ID',
```

Get a free project ID from [WalletConnect Cloud](https://cloud.walletconnect.com/).

## 📊 Smart Contract Interface

### Main Functions

#### `submitHealthData()`
Submit encrypted health metrics and calculate health score.

```solidity
function submitHealthData(
    externalEuint32 _bmi,
    bytes calldata _bmiProof,
    externalEuint32 _bloodSugar,
    bytes calldata _bloodSugarProof,
    externalEuint32 _heartRate,
    bytes calldata _heartRateProof
) external
```

#### `getBmi()`, `getBloodSugar()`, `getHeartRate()`, `getHealthScore()`
Retrieve encrypted health data for the caller.

```solidity
function getBmi() external view returns (euint32)
function getBloodSugar() external view returns (euint32)
function getHeartRate() external view returns (euint32)
function getHealthScore() external view returns (euint32)
```

#### `hasHealthData()`
Check if caller has submitted data.

```solidity
function hasHealthData() external view returns (bool)
```

#### `getTotalUsers()`
Get total number of users who have submitted data.

```solidity
function getTotalUsers() external view returns (uint256)
```

## 🌐 Deployment to Sepolia

### 1. Set Environment Variables

```bash
npx hardhat vars set MNEMONIC
npx hardhat vars set INFURA_API_KEY
npx hardhat vars set ETHERSCAN_API_KEY
```

### 2. Deploy to Sepolia

```bash
npm run deploy:sepolia
```

### 3. Update Frontend Config

Update `frontend/src/config/contract.ts` with the Sepolia contract address.

### 4. Run Sepolia Tests

```bash
npx hardhat test --network sepolia test/HealthMetricsSepolia.ts
```

## 🎨 Frontend Features

### Components

- **Header**: Displays logo and Rainbow wallet connection button
- **HealthDashboard**: Main dashboard with stats and data management
- **HealthDataForm**: Form for submitting encrypted health metrics
- **HealthDataDisplay**: Display and decrypt health data
- **StatsCard**: Reusable statistics card component

### UI Highlights

- Modern glassmorphism design
- Gradient borders and accents
- Responsive layout for all screen sizes
- Real-time transaction status updates
- Smooth animations and transitions

## 🔒 Security Considerations

1. **Encryption**: All sensitive data is encrypted before leaving the client
2. **Decryption Permissions**: Only data owners can decrypt their information
3. **On-Chain Privacy**: Health data stored on-chain is fully encrypted
4. **No Plaintext Leakage**: Contract never sees unencrypted values

## 🧩 Technology Stack

### Backend
- Solidity 0.8.27
- Hardhat
- FHEVM (Fully Homomorphic Encryption Virtual Machine)
- TypeScript

### Frontend
- React 18
- TypeScript
- Vite
- Tailwind CSS
- Rainbow Kit & Wagmi
- fhevmjs

## 📝 License

MIT License - see LICENSE file for details.

## 👥 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 🆘 Support

For issues, questions, or suggestions, please open an issue on GitHub.

## 🙏 Acknowledgments

- **Zama** for FHEVM technology
- **Rainbow Kit** for wallet connection UI
- **Hardhat** for development framework

## 📚 Additional Resources

- [Zama FHEVM Documentation](https://docs.zama.ai/fhevm)
- [Rainbow Kit Docs](https://www.rainbowkit.com/docs/introduction)
- [Hardhat Documentation](https://hardhat.org/docs)

---

**Built with ❤️ using Fully Homomorphic Encryption**

