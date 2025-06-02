# Package.json and Project Setup Files

## package.json
```json
{
  "name": "solana-flash-loan-bot",
  "version": "1.0.0",
  "description": "Automated Solana flash loan arbitrage bot with profit distribution",
  "main": "dist/index.js",
  "bin": {
    "flash-loan-bot": "dist/index.js"
  },
  "scripts": {
    "build": "tsc && anchor build",
    "start": "node dist/index.js",
    "dev": "ts-node src/index.ts",
    "test": "anchor test",
    "deploy": "anchor deploy",
    "simulate": "npm run dev -- --simulate",
    "init-pool": "npm run dev -- --init-pool",
    "lint": "eslint src/**/*.ts",
    "format": "prettier --write src/**/*.ts",
    "clean": "rm -rf dist target",
    "setup": "./scripts/setup.sh",
    "health": "curl -s http://localhost:8080/health | jq",
    "stats": "curl -s http://localhost:8080/stats | jq"
  },
  "dependencies": {
    "@coral-xyz/anchor": "^0.29.0",
    "@solana/web3.js": "^1.87.6",
    "@solana/spl-token": "^0.3.8",
    "@types/node": "^20.10.0",
    "axios": "^1.6.2",
    "chalk": "^4.1.2",
    "commander": "^11.1.0",
    "dotenv": "^16.3.1",
    "express": "^4.18.2",
    "winston": "^3.11.0",
    "bs58": "^5.0.0"
  },
  "devDependencies": {
    "@types/express": "^4.17.21",
    "@typescript-eslint/eslint-plugin": "^6.13.0",
    "@typescript-eslint/parser": "^6.13.0",
    "eslint": "^8.54.0",
    "prettier": "^3.1.0",
    "ts-node": "^10.9.1",
    "typescript": "^5.3.2",
    "nodemon": "^3.0.2"
  },
  "engines": {
    "node": ">=18.0.0",
    "npm": ">=8.0.0"
  },
  "keywords": [
    "solana",
    "flash-loan",
    "arbitrage",
    "defi",
    "trading-bot",
    "cryptocurrency",
    "automated-trading"
  ],
  "author": "Flash Loan Bot",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/your-username/solana-flash-loan-bot.git"
  },
  "bugs": {
    "url": "https://github.com/your-username/solana-flash-loan-bot/issues"
  },
  "homepage": "https://github.com/your-username/solana-flash-loan-bot#readme"
}
```

## tsconfig.json
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "lib": ["ES2020"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "removeComments": false,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "resolveJsonModule": true,
    "allowSyntheticDefaultImports": true,
    "moduleResolution": "node"
  },
  "include": [
    "src/**/*"
  ],
  "exclude": [
    "node_modules",
    "dist",
    "target",
    "tests"
  ]
}
```

## .env.example
```bash
# Solana Configuration
SOLANA_CLUSTER=devnet
DEVNET_RPC_URL=https://api.devnet.solana.com
TESTNET_RPC_URL=https://api.testnet.solana.com
MAINNET_RPC_URL=https://api.mainnet-beta.solana.com

# Wallet Configuration (Choose one method)
# Method 1: Private key as JSON array
PRIVATE_KEY=[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64]

# Method 2: Path to keypair file
KEYPAIR_PATH=~/.config/solana/id.json

# Beneficiary wallet address (receives profits)
BENEFICIARY_ADDRESS=YourWalletAddressHere

# Flash Loan Program Configuration
PROGRAM_ID=FLashLoanBotKXSr3w7W9mMUXLkiNqA8FhcEZMY2N8VkxNYDeryu
DEFAULT_FEE_BPS=50
SLIPPAGE_TOLERANCE_BPS=50

# Bot Configuration
LOG_LEVEL=info
LOOP_INTERVAL_MS=1000
MAX_CONCURRENT_REQUESTS=3
HEALTH_CHECK_PORT=8080

# Transaction Configuration
MAX_RETRIES=3
CONFIRMATION_TIMEOUT_MS=60000
COMPUTE_UNIT_LIMIT=1000000
COMPUTE_UNIT_PRICE=10000

# DEX Configuration
ENABLE_JUPITER=true
ENABLE_RAYDIUM=true
ENABLE_ORCA=true
ENABLE_SERUM=true

JUPITER_API_URL=https://quote-api.jup.ag/v6
RAYDIUM_API_URL=https://api.raydium.io
ORCA_API_URL=https://api.orca.so

# Arbitrage Configuration
MIN_PROFIT_THRESHOLD=10000
MIN_PROFITABILITY_BPS=50
MAX_POSITION_SIZE_USD=10000

# Risk Management
MAX_DAILY_TRANSACTIONS=1000
MAX_CONSECUTIVE_FAILURES=5
COOLDOWN_AFTER_FAILURES_MS=300000

# Notifications (Optional)
ENABLE_TELEGRAM_NOTIFICATIONS=false
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
TELEGRAM_CHAT_ID=your_telegram_chat_id

ENABLE_DISCORD_NOTIFICATIONS=false
DISCORD_WEBHOOK_URL=https://discord.com/api/webhooks/your_webhook_url

# Performance Monitoring
ENABLE_PERFORMANCE_METRICS=true
DATABASE_ENABLED=false
DATABASE_URL=postgres://user:password@localhost:5432/flashloanbot
```

## Cargo.toml (for Anchor program)
```toml
[package]
name = "flash-loan-bot"
version = "0.1.0"
description = "Solana Flash Loan Arbitrage Smart Contract"
edition = "2021"

[lib]
crate-type = ["cdylib", "lib"]
name = "flash_loan_bot"

[features]
no-entrypoint = []
no-idl = []
no-log-ix-name = []
cpi = ["no-entrypoint"]
default = []

[dependencies]
anchor-lang = "0.29.0"
anchor-spl = "0.29.0"
```

## .gitignore
```
# Dependencies
node_modules/
target/
dist/

# Environment variables
.env
.env.local
.env.production

# Logs
*.log
logs/

# Runtime data
pids/
*.pid
*.seed
*.pid.lock

# Coverage directory used by tools like istanbul
coverage/

# nyc test coverage
.nyc_output/

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Solana
test-ledger/
.anchor/

# Build outputs
*.tsbuildinfo

# Keypairs (NEVER commit real keypairs)
*.json
!package*.json
!tsconfig.json
!Anchor.toml
```

## README.md
```markdown
# Solana Flash Loan Arbitrage Bot

An automated arbitrage trading bot for Solana that uses flash loans to exploit price differences across DEXes, with automatic profit distribution to your specified wallet.

## Features

- 🚀 **Flash Loan Arbitrage**: Execute trades without upfront capital
- 💰 **Automatic Profit Distribution**: Profits sent directly to your wallet
- 🔄 **Multi-DEX Support**: Jupiter, Raydium, Orca, Serum
- 📊 **Real-time Monitoring**: Track performance and opportunities
- 🛡️ **Risk Management**: Built-in safety measures and limits
- 📱 **Notifications**: Telegram, Discord, Slack alerts
- 🖥️ **Web Interface**: Monitor bot status and statistics

## Quick Start

### 1. Installation

\`\`\`bash
# Clone the repository
git clone https://github.com/your-username/solana-flash-loan-bot.git
cd solana-flash-loan-bot

# Install dependencies
npm install

# Build the project
npm run build
\`\`\`

### 2. Configuration

\`\`\`bash
# Copy environment template
cp .env.example .env

# Edit configuration
nano .env
\`\`\`

**Required Environment Variables:**
- `BENEFICIARY_ADDRESS`: Your wallet address to receive profits
- `PRIVATE_KEY` or `KEYPAIR_PATH`: Bot wallet credentials
- `SOLANA_CLUSTER`: Network (devnet/mainnet-beta)

### 3. Initialize Flash Loan Pool

\`\`\`bash
# Initialize pool on devnet
npm run init-pool
\`\`\`

### 4. Run the Bot

\`\`\`bash
# Start production bot
npm start

# Or run in development mode
npm run dev

# Run simulation (no real transactions)
npm run simulate
\`\`\`

## Usage Examples

### Basic Usage
\`\`\`bash
# Start with default configuration
npm start

# Specify custom beneficiary
npm run dev -- --beneficiary YOUR_WALLET_ADDRESS

# Run on mainnet
SOLANA_CLUSTER=mainnet-beta npm start
\`\`\`

### Advanced Configuration
\`\`\`bash
# Verbose logging
npm run dev -- --verbose

# Custom network
npm run dev -- --network testnet

# Initialize pool with custom settings
npm run dev -- --init-pool --beneficiary YOUR_ADDRESS
\`\`\`

## Monitoring

### Health Check
\`\`\`bash
# Check bot health
curl http://localhost:8080/health

# Get detailed stats
curl http://localhost:8080/stats
\`\`\`

### Performance Metrics
- Success rate and profitability
- Transaction history
- Real-time opportunities
- Risk metrics

## Safety Features

- **Minimum Profit Thresholds**: Only execute profitable trades
- **Slippage Protection**: Prevent losses from price movements
- **Rate Limiting**: Avoid overwhelming DEX APIs
- **Circuit Breakers**: Auto-pause on consecutive failures
- **Emergency Controls**: Manual pause functionality

## Supported DEXes

- **Jupiter**: Aggregated routing for best prices
- **Raydium**: AMM with concentrated liquidity
- **Orca**: Concentrated liquidity pools
- **Serum**: Central limit order book

## Configuration Options

### Trading Parameters
- `MIN_PROFIT_THRESHOLD`: Minimum profit in lamports
- `MIN_PROFITABILITY_BPS`: Minimum profit percentage
- `MAX_POSITION_SIZE_USD`: Maximum trade size
- `SLIPPAGE_TOLERANCE_BPS`: Acceptable slippage

### Risk Management
- `MAX_DAILY_TRANSACTIONS`: Daily transaction limit
- `MAX_CONSECUTIVE_FAILURES`: Failure threshold
- `COOLDOWN_AFTER_FAILURES_MS`: Cooldown period

## Troubleshooting

### Common Issues

1. **Insufficient Balance**
   \`\`\`bash
   # Check wallet balance
   solana balance
   
   # Airdrop on devnet
   solana airdrop 2
   \`\`\`

2. **Connection Issues**
   \`\`\`bash
   # Test RPC connection
   solana cluster-version
   \`\`\`

3. **Program Not Found**
   \`\`\`bash
   # Deploy program
   npm run deploy
   \`\`\`

### Logs and Debugging
\`\`\`bash
# Enable debug logging
LOG_LEVEL=debug npm start

# Check logs in real-time
tail -f logs/bot.log
\`\`\`

## Security

⚠️ **Important Security Notes:**

- Never commit private keys to version control
- Use environment variables for sensitive data
- Test thoroughly on devnet before mainnet
- Monitor bot activity regularly
- Set appropriate risk limits

## License

MIT License - see LICENSE file for details

## Support

- GitHub Issues: Report bugs and request features
- Documentation: Comprehensive setup and usage guides
- Community: Join our Discord for support

## Disclaimer

This software is for educational purposes. Use at your own risk. Cryptocurrency trading involves substantial risk of loss.
\`\`\`
```