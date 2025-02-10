# Decentralized Stablecoin System

A minimal decentralized stablecoin system implemented in Solidity. This system is designed to be as simple as possible while ensuring that the stablecoin (DSC) remains fully overcollateralized. It is inspired by MakerDAO’s DAI but without governance, fees, or multiple collateral types beyond the basics.

The system includes:

- **DecentralizedStableCoin.sol**: An ERC20 token contract representing the stablecoin (DSC). This token is mintable and burnable only by the DSCEngine.
- **DSCEngine.sol**: The core engine that manages collateral deposits, minting, burning, collateral redemption, and liquidation of undercollateralized positions.
- **HelperConfig.s.sol**: A configuration contract that sets up network-specific parameters (like price feed and collateral token addresses). It deploys mocks when running on local networks (e.g., Anvil) and uses real addresses on testnets like Sepolia.
- **DeployDSC.s.sol**: A Foundry deployment script that deploys the DSC and DSCEngine contracts and configures them appropriately (including transferring the ownership of DSC to the DSCEngine).

---

## Table of Contents

- [Overview](#overview)
- [File Structure](#file-structure)
- [Installation](#installation)
- [Usage](#usage)
  - [Running Tests](#running-tests)
  - [Deploying Contracts](#deploying-contracts)
- [Contract Details](#contract-details)
- [Dependencies](#dependencies)
- [License](#license)
- [Acknowledgments](#acknowledgments)

---

## Overview

This repository implements a decentralized stablecoin system with the following features:

- **Overcollateralization**: The system enforces a minimum collateralization ratio to ensure stability.
- **Collateral Management**: Users can deposit collateral tokens (e.g., WETH and WBTC) and mint DSC against the value of their collateral.
- **Liquidation Mechanism**: Under-collateralized positions can be liquidated to bring the system back to a safe state. Liquidators receive a bonus on the collateral they claim.
- **Price Feeds Integration**: Uses Chainlink (or mock versions thereof) to fetch up-to-date collateral prices, ensuring accurate USD valuations.
- **Foundry Scripting**: Uses Foundry for deployment and testing, making it easy to deploy to various networks.

---

## File Structure

```plaintext
.
├── contracts
│   ├── DecentralizedStableCoin.sol    # ERC20 token contract for the stablecoin (DSC)
│   ├── DSCEngine.sol                  # Core engine for collateral management, minting/burning, and liquidation
│   └── libraries
│       └── OracleLib.sol              # Library for interacting with Chainlink (or mock) price feeds
├── script
│   ├── DeployDSC.s.sol                # Foundry deployment script for DSC and DSCEngine
│   └── HelperConfig.s.sol             # Configuration script for network-specific settings & mocks
├── test
│   └── ...                            # Test files (not provided here, but you can add your tests)
└── README.md                          # This file
```

---

## Installation

1. **Clone the Repository**

   ```bash
   git clone https://github.com/your-username/your-repo.git
   cd your-repo
   ```

2. **Install Foundry**

   This project uses [Foundry](https://github.com/foundry-rs/foundry) for development, testing, and deployment. If you haven’t installed Foundry yet, run:

   ```bash
   curl -L https://foundry.paradigm.xyz | bash
   foundryup
   ```

3. **Install Dependencies**

   The project leverages OpenZeppelin contracts and other libraries. You can install these dependencies using Foundry:

   ```bash
   forge install OpenZeppelin/openzeppelin-contracts
   ```

4. **Set Environment Variables**

   If deploying to a network like Sepolia, ensure you set the `PRIVATE_KEY` environment variable:

   ```bash
   export PRIVATE_KEY=your-private-key-here
   ```

---

## Usage

### Running Tests

You can run the tests (once you add them under the `test` directory) with:

```bash
forge test
```

### Deploying Contracts

Deploy the contracts using Foundry’s script runner. For example, to deploy to a testnet, run:

```bash
forge script script/DeployDSC.s.sol --broadcast --verify --rpc-url <YOUR_RPC_URL>
```

Replace `<YOUR_RPC_URL>` with the RPC URL of your chosen network (e.g., Infura, Alchemy).

---

## Contract Details

### DecentralizedStableCoin.sol

- **Purpose**: Implements an ERC20 stablecoin (DSC) that is mintable and burnable.
- **Key Features**:
  - Only the DSCEngine (owner) can mint or burn DSC.
  - Uses OpenZeppelin’s ERC20, ERC20Burnable, and Ownable for secure and standard token functionality.
  - Provides error handling to ensure valid transactions (e.g., burning more than the balance).

### DSCEngine.sol

- **Purpose**: Manages collateral deposits, minting, burning, collateral redemption, and liquidation.
- **Key Features**:
  - Enforces overcollateralization through a health factor mechanism.
  - Integrates with Chainlink price feeds (or mocks) to determine the USD value of deposited collateral.
  - Supports operations to deposit collateral and mint DSC, redeem collateral by burning DSC, and liquidate undercollateralized accounts.
  - Uses OpenZeppelin’s `ReentrancyGuard` to prevent reentrancy attacks.
  - Contains detailed NatSpec comments for clarity on each function’s purpose and usage.

### HelperConfig.s.sol

- **Purpose**: Provides configuration for the system based on the network.
- **Key Features**:
  - Sets up addresses for price feeds and collateral tokens.
  - Deploys mock contracts for local testing (e.g., on Anvil) if real addresses are not available.
  - Supports both local and testnet (e.g., Sepolia) configurations.

### DeployDSC.s.sol

- **Purpose**: A deployment script written with Foundry that automates the deployment process.
- **Key Features**:
  - Instantiates the HelperConfig to load the correct network configuration.
  - Deploys the DecentralizedStableCoin and DSCEngine contracts.
  - Transfers ownership of the DSC token to the DSCEngine, ensuring that only DSCEngine can mint or burn DSC.

---

## Dependencies

- **Foundry**: For compiling, testing, and deploying the contracts.
- **OpenZeppelin Contracts**: Provides secure implementations for ERC20 tokens, ownership, and reentrancy guards.
- **Chainlink Aggregator Interface**: For integrating with price feeds.
- **Mock Contracts**: (e.g., `MockV3Aggregator.sol`, `ERC20Mock`) used for local testing and development.

---

## License

This project is licensed under the [MIT License](LICENSE).

---

## Acknowledgments

- **MakerDAO**: For inspiring the design and mechanism behind overcollateralized stablecoins.
- **OpenZeppelin**: For their robust and secure smart contract implementations.
- **Foundry**: For providing an excellent framework for Ethereum development.
- **Chainlink**: For enabling decentralized price feeds that help maintain the system's stability.

---

Happy Coding! 🚀

Feel free to contribute, open issues, or provide feedback. Enjoy building on a decentralized future!
