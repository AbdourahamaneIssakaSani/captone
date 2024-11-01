# Project Setup Guide

This guide explains how to set up Avalanche, CouchDB, and configure the project environment to run the API and React frontend.

## Prerequisites

Ensure the following software is installed:
- [Node.js](https://nodejs.org/) and npm
- [Docker](https://www.docker.com/get-started) (for CouchDB)
- [Avalanche CLI](https://docs.avax.network/build/tutorials/platform/create-a-local-test-network) 

## Step 1: Set Up Avalanche Blockchain

### 1.1 Install and Configure Avalanche

If Avalanche CLI isn’t installed, follow [these instructions](https://docs.avax.network/build/tutorials/platform/create-a-local-test-network).

### 1.2 Create a Subnet and Blockchain

Open your terminal and run the following commands to create and configure your Avalanche subnet blockchain:

```bash
avalanche blockchain create <subnet-name>
```

When prompted, configure as follows:
- **VM Type**: Select `Subnet-EVM`
- **Use Default Values**: Choose `No`
- **Version**: `v0.6.10`
- **Chain ID**: `<your_chain_id>`
- **Token Symbol**: `<your_token_symbol>`
- **Custom Allocation**: Yes, and add addresses:
  - **Address 1**: `<address_1>`, **Amount**: `<amount_1>`
  - **Address 2**: `<address_2>`, **Amount**: `<amount_2>`
- **Supply Cap**: Hard-capped
- **Gas Price Configuration**: Use defaults
- **Transaction Fees**: Burn
- **Isolation**: Yes
- **Allow Anyone to Issue Transactions**: Yes
- **Smart Contract Deployment**: Only approved addresses
  - **Admin** Address: `<admin_address>`

### 1.3 Deploy the Blockchain

Deploy your configured blockchain to a local network:

```bash
avalanche blockchain deploy <subnet-name>
```

After deployment, note the **RPC URL** for the blockchain, which will look like:
```
http://127.0.0.1:9650/ext/bc/<subnet-name>/rpc
```

## Step 2: Deploy Smart Contract with Remix and MetaMask

1. Open [Remix IDE](https://remix.ethereum.org/).
2. Connect your MetaMask to the Avalanche network you created.
3. Compile and deploy your smart contract using Remix.
4. Once deployed, note the contract address for your `.env` file configuration.

## Step 3: Configure Environment Variables

In the root of your project, create a `.env` file with the following variables, updating values based on your setup:

```plaintext
JWT_ACCESS_SECRET=your_jwt_access_secret
JWT_REFRESH_SECRET=your_jwt_refresh_secret
COUCHDB_URL=http://localhost:5985
COUCHDB_USER=your_couchdb_user
COUCHDB_PASSWORD=your_couchdb_password
PORT=9000
ROOT_BLOCKCHAIN_ACCOUNT=your_root_blockchain_account
ROOT_BLOCKCHAIN_PRIVATE_KEY=your_root_blockchain_private_key
JWT_ACCESS_EXPIRES=1d
JWT_REFRESH_EXPIRES=2d
EMAIL_PASSWORD="your_email_password"
CONTRACT_ADDRESS=your_contract_address
RPC_URL=http://127.0.0.1:9650/ext/bc/<subnet-name>/rpc
```

Replace each placeholder with your actual configuration values.

## Step 4: Set Up CouchDB

Run CouchDB in a Docker container:

```bash
docker run -p 5985:5984 -d --name couchdb -e COUCHDB_USER=your_couchdb_user -e COUCHDB_PASSWORD=your_couchdb_password couchdb:latest
```

### Optional: Set Up Test Database

For testing, set up a separate CouchDB instance named `couchdbtest`:

```bash
docker run -p 5986:5984 -d --name couchdbtest -e COUCHDB_USER=your_couchdb_user -e COUCHDB_PASSWORD=your_couchdb_password couchdb:latest
```

## Step 5: Run the API

1. Install dependencies:
   ```bash
   cd vaxichain-api
   npm install
   ```

2. Start the API server:
   ```bash
   npm start
   ```

The API server will run on the port specified in your `.env` file (`PORT=9000`).

## Step 6: Run the React Frontend

1. Install dependencies:
   ```bash
   cd vaxichain
   npm install
   ```

2. Start the frontend:
   ```bash
   npm start
   ```

This will run the frontend on the default port `3000` or a different one if specified.

## Running Tests (Optional)

To run tests using the test CouchDB instance, ensure the database `couchdbtest` is running on `5986`.

## Summary

1. **Set up Avalanche and deploy the blockchain.**
2. **Deploy the smart contract with Remix and MetaMask.**
3. **Update the `.env` file with the correct values.**
4. **Set up CouchDB (and optional test database).**
5. **Run the API server.**
6. **Run the React frontend.**

This setup allows you to run the project API and frontend with Avalanche and CouchDB in a local environment.

