# Flash Loan Bot Configuration (config.ts)

```typescript
/**
 * Solana Flash Loan Bot Configuration
 * This file contains all the configuration options for the bot
 */
import { PublicKey } from "@solana/web3.js";

// Network configuration
export const NETWORK_CONFIG = {
  // Set to 'mainnet-beta', 'testnet', 'devnet', or 'localnet'
  CLUSTER: process.env.SOLANA_CLUSTER || "devnet",
  
  // RPC endpoint URLs
  RPC_ENDPOINTS: {
    "mainnet-beta": process.env.MAINNET_RPC_URL || "https://api.mainnet-beta.solana.com",
    "testnet": process.env.TESTNET_RPC_URL || "https://api.testnet.solana.com",
    "devnet": process.env.DEVNET_RPC_URL || "https://api.devnet.solana.com",
    "localnet": process.env.LOCALNET_RPC_URL || "http://localhost:8899",
  },
  
  // Optional backup RPC endpoints
  BACKUP_RPC_ENDPOINTS: {
    "mainnet-beta": [
      "https://solana-api.projectserum.com",
      "https://rpc.ankr.com/solana",
    ],
    "testnet": [],
    "devnet": [],
    "localnet": [],
  },
  
  // WebSocket endpoints for subscriptions
  WS_ENDPOINTS: {
    "mainnet-beta": process.env.MAINNET_WS_URL || "wss://api.mainnet-beta.solana.com",
    "testnet": process.env.TESTNET_WS_URL || "wss://api.testnet.solana.com",
    "devnet": process.env.DEVNET_WS_URL || "wss://api.devnet.solana.com",
    "localnet": process.env.LOCALNET_WS_URL || "ws://localhost:8900",
  },
};

// Bot configuration
export const BOT_CONFIG = {
  // Log level: 'debug', 'info', 'warn', 'error'
  LOG_LEVEL: process.env.LOG_LEVEL || "info",
  
  // Maximum number of concurrent requests
  MAX_CONCURRENT_REQUESTS: parseInt(process.env.MAX_CONCURRENT_REQUESTS || "3"),
  
  // Time between operation loops (in milliseconds)
  LOOP_INTERVAL_MS: parseInt(process.env.LOOP_INTERVAL_MS || "1000"),
  
  // Health check port for monitoring
  HEALTH_CHECK_PORT: parseInt(process.env.HEALTH_CHECK_PORT || "8080"),
  
  // Enable/disable various bot features
  FEATURES: {
    TELEGRAM_NOTIFICATIONS: process.env.ENABLE_TELEGRAM_NOTIFICATIONS === "true",
    SLACK_NOTIFICATIONS: process.env.ENABLE_SLACK_NOTIFICATIONS === "true",
    DISCORD_NOTIFICATIONS: process.env.ENABLE_DISCORD_NOTIFICATIONS === "true",
    EMAIL_NOTIFICATIONS: process.env.ENABLE_EMAIL_NOTIFICATIONS === "true",
    PERFORMANCE_METRICS: process.env.ENABLE_PERFORMANCE_METRICS === "true",
    AUTO_RETRY: process.env.ENABLE_AUTO_RETRY === "true",
    PROFIT_CALCULATOR: process.env.ENABLE_PROFIT_CALCULATOR !== "false",
    RISK_MANAGEMENT: process.env.ENABLE_RISK_MANAGEMENT !== "false",
    ERROR_REPORTING: process.env.ENABLE_ERROR_REPORTING !== "false",
  },
  
  // HTTP server configuration for the web interface
  HTTP_SERVER: {
    PORT: parseInt(process.env.HTTP_PORT || "3000"),
    HOST: process.env.HTTP_HOST || "localhost",
  },
  
  // Database configuration (if using)
  DATABASE: {
    ENABLED: process.env.DATABASE_ENABLED === "true",
    URL: process.env.DATABASE_URL || "postgres://user:password@localhost:5432/flashloanbot",
  },
};

// Wallet configuration
export const WALLET_CONFIG = {
  // Path to keypair file
  KEYPAIR_PATH: process.env.KEYPAIR_PATH || "~/.config/solana/id.json",
  
  // Optional: provide private key directly (higher priority than file)
  // WARNING: Never hardcode this in production, always use environment variables
  PRIVATE_KEY: process.env.PRIVATE_KEY || "",
  
  // Wallet to receive profits
  BENEFICIARY_ADDRESS: process.env.BENEFICIARY_ADDRESS || "",
};

// Flash loan program configuration
export const PROGRAM_CONFIG = {
  // Flash loan program ID
  PROGRAM_ID: new PublicKey(process.env.PROGRAM_ID || "FLashLoanBotKXSr3w7W9mMUXLkiNqA8FhcEZMY2N8VkxNYDeryu"),
  
  // Default fee in basis points (100 = 1%)
  DEFAULT_FEE_BPS: parseInt(process.env.DEFAULT_FEE_BPS || "50"),
  
  // Default slippage tolerance in basis points (100 = 1%)
  SLIPPAGE_TOLERANCE_BPS: parseInt(process.env.SLIPPAGE_TOLERANCE_BPS || "50"),
  
  // Program version (for compatibility checks)
  PROGRAM_VERSION: process.env.PROGRAM_VERSION || "1.0.0",
};

// Transaction configuration
export const TRANSACTION_CONFIG = {
  // Maximum number of transaction retries
  MAX_RETRIES: parseInt(process.env.MAX_RETRIES || "3"),
  
  // Retry backoff in milliseconds
  RETRY_BACKOFF_MS: parseInt(process.env.RETRY_BACKOFF_MS || "500"),
  
  // Confirmation commitment level: 'processed', 'confirmed', 'finalized'
  COMMITMENT: (process.env.COMMITMENT || "confirmed") as "processed" | "confirmed" | "finalized",
  
  // Timeout for transaction confirmation (in milliseconds)
  CONFIRMATION_TIMEOUT_MS: parseInt(process.env.CONFIRMATION_TIMEOUT_MS || "60000"),
  
  // Skip preflight for transactions
  SKIP_PREFLIGHT: process.env.SKIP_PREFLIGHT === "true",
  
  // Compute unit limit
  COMPUTE_UNIT_LIMIT: parseInt(process.env.COMPUTE_UNIT_LIMIT || "1000000"),
  
  // Compute unit price (in micro-lamports)
  COMPUTE_UNIT_PRICE: parseInt(process.env.COMPUTE_UNIT_PRICE || "10000"),
};

// DEX configuration for arbitrage
export const DEX_CONFIG = {
  // Supported DEXes
  SUPPORTED_DEXES: {
    // Raydium configuration
    RAYDIUM: {
      ENABLED: process.env.ENABLE_RAYDIUM !== "false",
      PROGRAM_ID: new PublicKey("675kPX9MHTjS2zt1qfr1NYHuzeLXfQM9H24wFSUt1Mp8"),
      API_URL: process.env.RAYDIUM_API_URL || "https://api.raydium.io",
      FEE_BPS: parseInt(process.env.RAYDIUM_FEE_BPS || "30"),
    },
    
    // Orca configuration
    ORCA: {
      ENABLED: process.env.ENABLE_ORCA !== "false",
      PROGRAM_ID: new PublicKey("9W959DqEETiGZocYWCQPaJ6sBmUzgfxXfqGeTEdp3aQP"),
      API_URL: process.env.ORCA_API_URL || "https://api.orca.so",
      FEE_BPS: parseInt(process.env.ORCA_FEE_BPS || "30"),
    },
    
    // Jupiter configuration
    JUPITER: {
      ENABLED: process.env.ENABLE_JUPITER !== "false",
      PROGRAM_ID: new PublicKey("JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4"),
      API_URL: process.env.JUPITER_API_URL || "https://quote-api.jup.ag/v6",
      FEE_BPS: parseInt(process.env.JUPITER_FEE_BPS || "30"),
      ROUTE_API_VERSION: process.env.JUPITER_ROUTE_API_VERSION || "v6",
    },
    
    // Serum configuration
    SERUM: {
      ENABLED: process.env.ENABLE_SERUM !== "false",
      PROGRAM_ID: new PublicKey("9xQeWvG816bUx9EPjHmaT23yvVM2ZWbrrpZb9PusVFin"),
      API_URL: process.env.SERUM_API_URL || "https://api.projectserum.com",
      FEE_BPS: parseInt(process.env.SERUM_FEE_BPS || "20"),
    },
  },
};

// Token configuration
export const TOKEN_CONFIG = {
  // Native SOL token
  NATIVE_SOL: {
    MINT: new PublicKey("So11111111111111111111111111111111111111112"),
    SYMBOL: "SOL",
    DECIMALS: 9,
  },
  
  // USDC token
  USDC: {
    MINT: new PublicKey("EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"),
    SYMBOL: "USDC",
    DECIMALS: 6,
  },
  
  // USDT token
  USDT: {
    MINT: new PublicKey("Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB"),
    SYMBOL: "USDT",
    DECIMALS: 6,
  },
  
  // Additional tokens to monitor
  ADDITIONAL_TOKENS: [
    {
      MINT: new PublicKey("4k3Dyjzvzp8eMZWUXbBCjEvwSkkk59S5iCNLY3QrkX6R"),
      SYMBOL: "RAY",
      DECIMALS: 6,
    },
    {
      MINT: new PublicKey("orcaEKTdK7LKz57vaAYr9QeNsVEPfiu6QeMU1kektZE"),
      SYMBOL: "ORCA",
      DECIMALS: 6,
    },
  ],
};

// Arbitrage configuration
export const ARBITRAGE_CONFIG = {
  // Minimum profit threshold in lamports
  MIN_PROFIT_THRESHOLD: parseInt(process.env.MIN_PROFIT_THRESHOLD || "10000"), // 0.00001 SOL
  
  // Minimum profitability percentage (after fees)
  MIN_PROFITABILITY_BPS: parseInt(process.env.MIN_PROFITABILITY_BPS || "50"), // 0.5%
  
  // Maximum flash loan amount (as percentage of available liquidity)
  MAX_LOAN_PERCENTAGE: parseInt(process.env.MAX_LOAN_PERCENTAGE || "90"), // 90%
  
  // Maximum position size in USD
  MAX_POSITION_SIZE_USD: parseInt(process.env.MAX_POSITION_SIZE_USD || "10000"), // $10,000
  
  // Path finding settings
  PATH_FINDING: {
    MAX_HOPS: parseInt(process.env.MAX_HOPS || "3"),
    MAX_RESULTS: parseInt(process.env.MAX_RESULTS || "5"),
    TIMEOUT_MS: parseInt(process.env.PATH_FINDING_TIMEOUT_MS || "2000"),
  },
  
  // Market monitoring settings
  MARKET_MONITORING: {
    POLL_INTERVAL_MS: parseInt(process.env.MARKET_POLL_INTERVAL_MS || "1000"),
    PAIRS_PER_BATCH: parseInt(process.env.PAIRS_PER_BATCH || "10"),
    USE_WEBSOCKETS: process.env.USE_WEBSOCKETS === "true",
  },
  
  // Risk management settings
  RISK_MANAGEMENT: {
    MAX_DAILY_TRANSACTIONS: parseInt(process.env.MAX_DAILY_TRANSACTIONS || "1000"),
    MAX_DAILY_VOLUME_USD: parseInt(process.env.MAX_DAILY_VOLUME_USD || "100000"),
    MAX_CONSECUTIVE_FAILURES: parseInt(process.env.MAX_CONSECUTIVE_FAILURES || "5"),
    COOLDOWN_AFTER_FAILURES_MS: parseInt(process.env.COOLDOWN_AFTER_FAILURES_MS || "300000"),
  },
};

// Gas/Priority fee bidding strategies
export const FEE_BIDDING_CONFIG = {
  // Strategy: 'fixed', 'dynamic', 'aggressive'
  STRATEGY: process.env.FEE_BIDDING_STRATEGY || "dynamic",
  
  // Fixed priority fee (micro-lamports)
  FIXED_PRIORITY_FEE: parseInt(process.env.FIXED_PRIORITY_FEE || "10000"),
  
  // Dynamic fee settings (for dynamic strategy)
  DYNAMIC: {
    BASE_FEE: parseInt(process.env.DYNAMIC_BASE_FEE || "5000"),
    MAX_FEE: parseInt(process.env.DYNAMIC_MAX_FEE || "1000000"),
    MULTIPLIER: parseFloat(process.env.DYNAMIC_MULTIPLIER || "2"),
    PROFITABILITY_THRESHOLD_MULTIPLIER: parseFloat(process.env.PROFITABILITY_THRESHOLD_MULTIPLIER || "0.1"),
  },
  
  // Aggressive fee settings (for aggressive strategy)
  AGGRESSIVE: {
    BASE_FEE: parseInt(process.env.AGGRESSIVE_BASE_FEE || "100000"),
    MAX_FEE: parseInt(process.env.AGGRESSIVE_MAX_FEE || "5000000"),
    PROFIT_PERCENTAGE: parseFloat(process.env.AGGRESSIVE_PROFIT_PERCENTAGE || "0.2"),
  },
};

// Notification settings
export const NOTIFICATION_CONFIG = {
  // Telegram settings
  TELEGRAM: {
    BOT_TOKEN: process.env.TELEGRAM_BOT_TOKEN || "",
    CHAT_ID: process.env.TELEGRAM_CHAT_ID || "",
  },
  
  // Discord settings
  DISCORD: {
    WEBHOOK_URL: process.env.DISCORD_WEBHOOK_URL || "",
  },
  
  // Slack settings
  SLACK: {
    WEBHOOK_URL: process.env.SLACK_WEBHOOK_URL || "",
  },
  
  // Email settings
  EMAIL: {
    SMTP_HOST: process.env.SMTP_HOST || "",
    SMTP_PORT: parseInt(process.env.SMTP_PORT || "587"),
    SMTP_USER: process.env.SMTP_USER || "",
    SMTP_PASS: process.env.SMTP_PASS || "",
    FROM_ADDRESS: process.env.EMAIL_FROM_ADDRESS || "",
    TO_ADDRESS: process.env.EMAIL_TO_ADDRESS || "",
  },
};

// All configuration as a single export
export default {
  NETWORK: NETWORK_CONFIG,
  BOT: BOT_CONFIG,
  WALLET: WALLET_CONFIG,
  PROGRAM: PROGRAM_CONFIG,
  TRANSACTION: TRANSACTION_CONFIG,
  DEX: DEX_CONFIG,
  TOKEN: TOKEN_CONFIG,
  ARBITRAGE: ARBITRAGE_CONFIG,
  FEE_BIDDING: FEE_BIDDING_CONFIG,
  NOTIFICATION: NOTIFICATION_CONFIG,
};
```