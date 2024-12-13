# Rune/BRC20 Marketplace

Welcome to the **Rune/BRC20 Marketplace**, a decentralized application (dApp) built in the Bitcoin space. This project leverages **React** and **Bitcoin CLI** to facilitate the swapping of Rune/BRC20 tokens, providing an efficient platform for users to interact with pools and exchange assets.

Explore this repository to learn more about how it works, how it handles transaction processes, and how you can contribute!

## Introduction

This project allows users to create **Rune/BRC20 token pair pools** and swap tokens within them. When a user swaps, the pool gets locked to prevent others from accessing it until the operation is completed.

### Key Features:

1. **Pool Locking:** When a user performs a swap, the pool lock status is updated to prevent concurrent operations.
2. **UTXO Management:** Ensures that used transaction outputs (UTXOs) are stored and tracked to avoid confusion when fetching UTXOs for transactions.
3. **BRC20 Inscription Validation:** Before transferring BRC20 tokens, it checks if the address holds transferable inscription data.

## Project Setup

### Prerequisites

Before running the project, ensure that you have the following dependencies installed:

- **Node.js** (>= v18)
- **npm** or **yarn**

### Install Dependencies

1. Clone the repository:
   ```bash
   git clone https://github.com/ptc-bink/rune-brc20-marketplace
   ```

2. Navigate to the project directory:
   ```bash
   cd rune-brc20-marketplace
   ```

3. Install the required dependencies:
   ```bash
   npm install
   ```

### Development Scripts

The project comes with several useful npm scripts for development, building, and managing the project.

- **`npm run dev`**: Starts the development server using `ts-node-dev` to watch for changes in TypeScript files.
  ```bash
  npm run dev
  ```

- **`npm run build`**: Compiles the TypeScript code into JavaScript using the `tsc` (TypeScript Compiler).
  ```bash
  npm run build
  ```

- **`npm run check-types`**: Runs TypeScript type-checking to ensure there are no type errors in the project.
  ```bash
  npm run check-types
  ```

- **`npm run check-format`**: Checks the code formatting using **Prettier**.
  ```bash
  npm run check-format
  ```

- **`npm run format`**: Automatically formats the code according to the Prettier configuration.
  ```bash
  npm run format
  ```

### Project Structure

- **`src/`**: Contains all the source code for the application.
- **`network.config.ts`**: Configuration for network-related values (e.g., RPC URLs, wallet addresses).
- **`models/`**: Contains database models for swap history, used transactions, and pool information.
- **`utils/`**: Contains utility functions like handling BRC20 inscriptions, filtering UTXOs, etc.

### Configuration

All necessary configuration values, such as API keys, addresses, and network details, are located in **`network.config.ts`**.

### Example Code Snippets

#### Pool Locking:

When a user performs a swap, the pool lock status is updated to prevent concurrent swaps.

```javascript
// Update pool lock status as false if pool and lockedByAddress match
export const updatePoolLockStatus = async (
  poolAddress: string,
  tokenType: string,
  userAddress: string
) => {
  // Implementation as described above
};
```

#### UTXO Management:

To avoid using already claimed UTXOs, we store used transaction information and filter out used UTXOs when fetching them:

```javascript
// Model to store used transaction information
const UsedTxInfo = new mongoose.Schema({
  txid: { type: String, required: true },
  confirmedTx: { type: String, required: true },
  createdAt: { type: Date, default: new Date(new Date().toUTCString()) },
});
const UsedTxInfoModal = mongoose.model("UsedTxInfo", UsedTxInfo);
export default UsedTxInfoModal;
```

#### BRC20 Inscription Validation:

For BRC20 token transfers, it checks if the user's address has the required transferable inscription data:

```javascript
export const getBrc20TransferableInscriptionUtxoByAddress = async (address: string, ticker: string) => {
  const url = `${OPENAPI_UNISAT_URL}/v1/indexer/address/${address}/brc20/${ticker}/transferable-inscriptions`;
  const config = {
    headers: {
      Authorization: `Bearer ${OPENAPI_UNISAT_TOKEN}`,
    },
  };

  const inscriptionList = (await axios.get(url, config)).data.data.detail;

  return inscriptionList;
};
```


#### Swap Hisotry Model:

```javascript
const SwapHistory = new mongoose.Schema({
	poolAddress: { type: String, required: true },
	userAddress: { type: String, required: true },
	tokenType: { type: String, required: true },
	txId: { type: String, required: true },
	tokenAmount: { type: Number, required: true },
	btcAmount: { type: Number, required: true },
	swapType: { type: Number, required: true },
	createdAt: { type: Date, default: new Date(new Date().toUTCString()) },
});

const SwapHistoryModal = mongoose.model("SwapHistory", SwapHistory);

export default SwapHistoryModal;
```

## Dependencies

The project uses the following dependencies:

- **`@bitcoinerlab/secp256k1`**: A library for elliptic curve cryptography, used in Bitcoin transactions.
- **`@mempool/mempool.js`**: A library for interacting with Bitcoin's mempool.
- **`@ordjs/runestone`**: A library for interacting with Rune tokens on the Bitcoin network.
- **`axios`**: Promise-based HTTP client for making API requests.
- **`mongoose`**: MongoDB object modeling tool.
- **`express`**: Web framework for Node.js, used for handling HTTP requests.

## Contribution

We welcome contributions to improve the functionality and security of this project. To contribute, please fork the repository, make your changes, and create a pull request. Be sure to follow our coding standards and include tests for your changes.
