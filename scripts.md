# Setup and Deployment Scripts

## setup.sh
```bash
#!/bin/bash

# Solana Flash Loan Bot Setup Script
# This script installs all necessary dependencies and prepares the environment
# for running the flash loan bot.

set -e # Exit on any error

# Print header
echo "============================================================="
echo "          Solana Flash Loan Bot - Setup Script"
echo "============================================================="

# Check if running as root (not recommended)
if [ "$(id -u)" = "0" ]; then
  echo "⚠️  Warning: Running as root is not recommended!"
  read -p "Continue anyway? (y/n) " -n 1 -r
  echo
  if [[ ! $REPLY =~ ^[Yy]$ ]]; then
    echo "Setup aborted."
    exit 1
  fi
fi

# Check system requirements
echo "Checking system requirements..."

# Check Node.js installation
if command -v node &> /dev/null; then
  NODE_VERSION=$(node -v | cut -d'v' -f2)
  echo "✓ Node.js found: v$NODE_VERSION"
  if [[ "${NODE_VERSION%%.*}" -lt 18 ]]; then
    echo "⚠️  Node.js version too old, minimum required is 18.x"
    echo "Please upgrade Node.js and try again"
    exit 1
  fi
else
  echo "✗ Node.js not found!"
  echo "Please install Node.js v18 or later and try again"
  exit 1
fi

# Check npm installation
if command -v npm &> /dev/null; then
  NPM_VERSION=$(npm -v)
  echo "✓ npm found: v$NPM_VERSION"
else
  echo "✗ npm not found!"
  echo "Please install npm and try again"
  exit 1
fi

# Check Solana CLI installation
if command -v solana &> /dev/null; then
  SOLANA_VERSION=$(solana --version | head -n1 | cut -d' ' -f2)
  echo "✓ Solana CLI found: $SOLANA_VERSION"
else
  echo "✗ Solana CLI not found!"
  echo "Installing Solana CLI..."
  
  # Install Solana CLI
  sh -c "$(curl -sSfL https://release.solana.com/v1.16.17/install)"
  export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"
  
  if command -v solana &> /dev/null; then
    echo "✓ Solana CLI installed successfully"
  else
    echo "✗ Failed to install Solana CLI"
    echo "Please install manually and try again: https://docs.solanalabs.com/cli/install"
    exit 1
  fi
fi

# Check Anchor installation
if command -v anchor &> /dev/null; then
  ANCHOR_VERSION=$(anchor --version | cut -d' ' -f2)
  echo "✓ Anchor CLI found: v$ANCHOR_VERSION"
else
  echo "✗ Anchor CLI not found!"
  echo "Installing Anchor CLI..."
  
  # Install Anchor CLI
  cargo install --git https://github.com/coral-xyz/anchor anchor-cli --locked
  
  if command -v anchor &> /dev/null; then
    echo "✓ Anchor CLI installed successfully"
  else
    echo "✗ Failed to install Anchor CLI"
    echo "Please install manually and try again: https://www.anchor-lang.com/docs/installation"
    exit 1
  fi
fi

# Install project dependencies
echo "Installing project dependencies..."
npm install

# Build the project
echo "Building the project..."
npm run build

# Create .env file if it doesn't exist
if [ ! -f .env ]; then
  echo "Creating .env configuration file..."
  cp .env.example .env
  echo "✓ Created .env file"
  echo "⚠️  Please edit .env with your configuration"
fi

# Check for Solana wallet
echo "Checking for Solana wallet..."
if [ ! -f ~/.config/solana/id.json ]; then
  echo "No Solana wallet found, creating one..."
  mkdir -p ~/.config/solana
  solana-keygen new --no-passphrase -o ~/.config/solana/id.json
  PUBKEY=$(solana-keygen pubkey ~/.config/solana/id.json)
  echo "✓ Created new Solana wallet: $PUBKEY"
else
  PUBKEY=$(solana-keygen pubkey ~/.config/solana/id.json)
  echo "✓ Found existing Solana wallet: $PUBKEY"
fi

# Set Solana config
echo "Configuring Solana CLI..."
solana config set --url https://api.devnet.solana.com
echo "✓ Set Solana cluster to devnet"

# Check if user wants to airdrop SOL on devnet
read -p "Do you want to airdrop 2 SOL to your wallet on devnet? (y/n) " -n 1 -r
echo
if [[ $REPLY =~ ^[Yy]$ ]]; then
  echo "Requesting airdrop..."
  solana airdrop 2
  sleep 2
  BALANCE=$(solana balance)
  echo "✓ Wallet balance: $BALANCE"
fi

# Update .env file with wallet info
if grep -q "BENEFICIARY_ADDRESS=" .env; then
  if grep -q "BENEFICIARY_ADDRESS=YourWalletAddressHere" .env; then
    echo "Updating beneficiary address in .env..."
    sed -i.bak "s/BENEFICIARY_ADDRESS=YourWalletAddressHere/BENEFICIARY_ADDRESS=$PUBKEY/" .env
    rm .env.bak 2>/dev/null || true
  fi
fi

# Deploy program to devnet (optional)
read -p "Do you want to deploy the program to devnet? (y/n) " -n 1 -r
echo
if [[ $REPLY =~ ^[Yy]$ ]]; then
  echo "Deploying program to devnet..."
  npm run deploy
  
  # Update program ID in .env
  PROGRAM_ID=$(solana address -k target/deploy/flash_loan_bot-keypair.json)
  if [ ! -z "$PROGRAM_ID" ]; then
    echo "Updating program ID in .env..."
    sed -i.bak "s/PROGRAM_ID=FLashLoanBotKXSr3w7W9mMUXLkiNqA8FhcEZMY2N8VkxNYDeryu/PROGRAM_ID=$PROGRAM_ID/" .env
    rm .env.bak 2>/dev/null || true
  fi
fi

# Initialize pool (optional)
read -p "Do you want to initialize the flash loan pool? (y/n) " -n 1 -r
echo
if [[ $REPLY =~ ^[Yy]$ ]]; then
  echo "Initializing flash loan pool..."
  npm run init-pool
fi

# Setup complete
echo "============================================================="
echo "✓ Setup completed successfully!"
echo "============================================================="
echo ""
echo "Next steps:"
echo "1. Edit the .env file to customize your configuration"
echo "2. Start the bot with: npm start"
echo "3. Run in simulation mode: npm run simulate"
echo ""
echo "For more information, read the README.md file"
echo "============================================================="

exit 0
```

## deploy.sh
```bash
#!/bin/bash

# Solana Flash Loan Bot Deployment Script
# This script builds and deploys the flash loan program to Solana

set -e # Exit on any error

# Print header
echo "============================================================="
echo "          Solana Flash Loan Bot - Deployment Script"
echo "============================================================="

# Load configuration
source .env 2>/dev/null || true

# Determine Solana cluster
CLUSTER=${SOLANA_CLUSTER:-"devnet"}
echo "Deploying to cluster: $CLUSTER"

# Build program
echo "Building program..."
npm run build

# Deploy program
echo "Deploying program to $CLUSTER..."

if [ "$CLUSTER" = "localnet" ]; then
  # Check if local validator is running
  if ! pgrep -x "solana-test-validator" > /dev/null; then
    echo "Local validator not running. Starting solana-test-validator..."
    solana-test-validator --reset &
    sleep 5
  fi
  
  # Set config to local
  solana config set --url http://localhost:8899
elif [ "$CLUSTER" = "devnet" ]; then
  solana config set --url https://api.devnet.solana.com
elif [ "$CLUSTER" = "testnet" ]; then
  solana config set --url https://api.testnet.solana.com
elif [ "$CLUSTER" = "mainnet-beta" ]; then
  solana config set --url https://api.mainnet-beta.solana.com
  
  # Safety check for mainnet
  echo "⚠️  WARNING: You are deploying to MAINNET!"
  read -p "Are you absolutely sure you want to continue? (yes/no) " -r
  echo
  if [[ ! $REPLY =~ ^yes$ ]]; then
    echo "Deployment aborted."
    exit 1
  fi
else
  echo "Unknown cluster: $CLUSTER"
  exit 1
fi

# Deploy with Anchor
anchor deploy

# Get program ID
PROGRAM_ID=$(solana address -k target/deploy/flash_loan_bot-keypair.json)
echo "Program deployed with ID: $PROGRAM_ID"

# Update .env file with program ID if it exists
if [ -f .env ]; then
  echo "Updating program ID in .env..."
  sed -i.bak "s/PROGRAM_ID=.*$/PROGRAM_ID=$PROGRAM_ID/" .env
  rm .env.bak 2>/dev/null || true
fi

# Initialize pool if requested
read -p "Do you want to initialize the flash loan pool? (y/n) " -n 1 -r
echo
if [[ $REPLY =~ ^[Yy]$ ]]; then
  echo "Initializing flash loan pool..."
  npm run init-pool
fi

echo "============================================================="
echo "✓ Deployment completed successfully!"
echo "Program ID: $PROGRAM_ID"
echo "============================================================="

exit 0
```

## test.sh
```bash
#!/bin/bash

# Solana Flash Loan Bot Test Script
# This script runs tests for the flash loan bot

set -e # Exit on any error

# Print header
echo "============================================================="
echo "          Solana Flash Loan Bot - Test Script"
echo "============================================================="

# Determine test mode
if [ "$1" = "--unit" ]; then
  TEST_MODE="unit"
elif [ "$1" = "--integration" ]; then
  TEST_MODE="integration"
elif [ "$1" = "--simulation" ]; then
  TEST_MODE="simulation"
else
  TEST_MODE="all"
fi

echo "Running tests in mode: $TEST_MODE"

# Run unit tests
if [ "$TEST_MODE" = "unit" ] || [ "$TEST_MODE" = "all" ]; then
  echo "Running unit tests..."
  # Add specific unit test command if needed
  npm test
fi

# Run integration tests
if [ "$TEST_MODE" = "integration" ] || [ "$TEST_MODE" = "all" ]; then
  echo "Running integration tests..."
  # Add specific integration test command if needed
  anchor test
fi

# Run simulation
if [ "$TEST_MODE" = "simulation" ] || [ "$TEST_MODE" = "all" ]; then
  echo "Running simulation..."
  npm run simulate
fi

echo "============================================================="
echo "✓ All tests completed successfully!"
echo "============================================================="

exit 0
```