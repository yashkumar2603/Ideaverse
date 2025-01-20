made for Sperax full stack role task

---

# Token Watchlist SPA

## Project Overview

The **Token Watchlist SPA** is a single-page application (SPA) designed to interact with the Ethereum blockchain, allowing users to:

1. **Add Tokens to a Watchlist:** Users can add various ERC-20 tokens to their personal watchlist.
2. **View Current Balances:** Display the current balance of each token in the watchlist.
3. **View Historical Balances:** Fetch and display historical token balances based on user-selected dates.
4. **Check Token Allowance:** View token allowances for different smart contracts.
5. **Perform Token Operations:** Transfer tokens to another address and approve token allowances.

### Live Demo

[Live Demo Link](https://token-watch.vercel.app/) 
## Table of Contents

- [Project Overview](#project-overview)
- [Setup Instructions](#setup-instructions)
- [Features](#features)
  - [Wallet Connection](#wallet-connection)
  - [Token Watchlist](#token-watchlist)
  - [Historical Data](#historical-data)
  - [Allowance Check](#allowance-check)
  - [Token Transfer](#token-transfer)
  - [Visual Representations](#visual-representations)
- [Technical Details](#technical-details)
  - [Blockchain Interaction](#blockchain-interaction)
  - [Error Handling](#error-handling)
- [Contributing](#contributing)
- [License](#license)

## Project Overview
Folder Structure - 
```
/my-token-app
│
├── /public
│   └── index.html
│
├── /src
│   ├── /assets
│   │   └── /images  (optional: to store images and other static assets)
│   │
│   ├── /components
│   │   ├── WalletConnector.js
│   │   ├── TokenList.js
│   │   ├── TokenItem.js
│   │   ├── HistoricalBalanceChart.js
│   │   ├── AllowanceChecker.js
│   │   └── TransferTokenForm.js
│   │
│   ├── /hooks
│   │   └── useWallet.js
│   │
│   ├── /services
│   │   ├── api.js
│   │   └── blockchain.js
│   │
│   ├── /pages
│   │   ├── Home.js
│   │   └── TokenDetails.js
│   │
│   ├── /styles
│   │   └── App.css
│   │
│   ├── App.js
│   ├── index.js
│   └── config.js
│
├── .gitignore
├── package.json
└── README.md
```

**Frameworks used** - ReactJS with Vite 
**Libraries used** - Web3.js, Chart.js 
**API** - Infura mainnet API
## Setup Instructions

### Prerequisites

- **Node.js** and **npm**: Ensure you have Node.js installed on your system. If not, download it from [here](https://nodejs.org/).
- **MetaMask**: Install the MetaMask browser extension or have an Ethereum wallet ready.

### Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/token-watchlist-spa.git
   cd token-watchlist-spa
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Environment Configuration**:
   - Create a `.env` file in the root directory with the following content:
     ```bash
     VITE_INFURA_PROJECT_ID=your-infura-project-id
     VITE_NETWORK=homestead
     ```
   - Replace `your-infura-project-id` with your actual Infura project ID.

4. **Start the development server**:
   ```bash
   npm run dev
   ```
   The application should be accessible at `http://localhost:5173`.

### Deployment

To deploy the application, you can use platforms like Netlify or Vercel. Follow their respective documentation for deployment steps.

## Features

### Wallet Connection

- Users can connect their MetaMask wallet, allowing the application to access their Ethereum account.
- Alternatively, users can manually input their wallet address.
- Upon connecting, the application verifies the network, ensuring the user is on the Ethereum Mainnet.

### Token Watchlist

- Users can add ERC-20 tokens to their watchlist by inputting the token's contract address.
- The application fetches and displays the token's name, symbol, and current balance.

### Historical Data

- Users can view the historical balance of each token by selecting a date range.
- Historical data is visually represented using graphs and charts for better user experience.

### Allowance Check

- The application allows users to check the allowance of a token for different smart contracts.
- This feature helps users understand which contracts can spend their tokens.

### Token Transfer

- Users can transfer tokens to another Ethereum address directly from the application.
- The application provides form fields to input the recipient's address and the amount to be transferred.

### Visual Representations

- The application uses tables, charts, and graphs to visually represent token balances, historical data, and allowances.
- Visual elements are designed to enhance user experience and make the data easily understandable.

## Technical Details

### Blockchain Interaction

- **Web3.js**: The application interacts with the Ethereum blockchain using Web3.js.
- **Infura**: Infura is used as the Ethereum node provider to facilitate network interactions without running a full node.
- **ERC-20 ABI**: The application uses a minimal ERC-20 ABI to interact with token contracts, focusing on essential functions like `name`, `symbol`, `balanceOf`, `allowance`, and `transfer`.

### Error Handling

- The application includes robust error handling to manage cases like invalid token addresses, network mismatches, and user-rejected transactions.
- Error messages are displayed in a user-friendly manner, ensuring a smooth experience.
