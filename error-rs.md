# Flash Loan Bot Errors (error.rs)

```rust
use anchor_lang::prelude::*;

#[error_code]
pub enum FlashLoanError {
    #[msg("Pool is currently paused")]
    PoolPaused,
    
    #[msg("Invalid loan amount")]
    InvalidAmount,
    
    #[msg("Insufficient liquidity in pool")]
    InsufficientLiquidity,
    
    #[msg("Loan amount exceeds maximum allowed")]
    AmountTooLarge,
    
    #[msg("Expected profit is below minimum threshold")]
    InsufficientProfit,
    
    #[msg("Loan was not repaid with required fee")]
    InsufficientRepayment,
    
    #[msg("No valid arbitrage instructions found in transaction")]
    NoArbitrageInstructions,
    
    #[msg("Unauthorized access")]
    Unauthorized,
    
    #[msg("Invalid fee percentage")]
    InvalidFee,
    
    #[msg("Insufficient fees available for withdrawal")]
    InsufficientFees,
    
    #[msg("Slippage tolerance exceeded")]
    SlippageExceeded,
    
    #[msg("Invalid DEX program ID")]
    InvalidDexProgram,
    
    #[msg("Arbitrage execution failed")]
    ArbitrageExecutionFailed,
    
    #[msg("Invalid beneficiary address")]
    InvalidBeneficiary,
    
    #[msg("Pool already initialized")]
    PoolAlreadyInitialized,
    
    #[msg("Invalid instruction index")]
    InvalidInstructionIndex,
    
    #[msg("Instruction introspection failed")]
    InstructionIntrospectionFailed,
    
    #[msg("Mathematical overflow")]
    MathematicalOverflow,
    
    #[msg("Account validation failed")]
    AccountValidationFailed,
    
    #[msg("Invalid token mint")]
    InvalidTokenMint,
    
    #[msg("Token account ownership mismatch")]
    TokenAccountOwnershipMismatch,
    
    #[msg("Insufficient token balance")]
    InsufficientTokenBalance,
    
    #[msg("Invalid swap instruction")]
    InvalidSwapInstruction,
    
    #[msg("Deadline exceeded")]
    DeadlineExceeded,
    
    #[msg("Price impact too high")]
    PriceImpactTooHigh,
    
    #[msg("Minimum output not met")]
    MinimumOutputNotMet,
    
    #[msg("Maximum input exceeded")]
    MaximumInputExceeded,
    
    #[msg("Invalid oracle price")]
    InvalidOraclePrice,
    
    #[msg("Oracle price too stale")]
    OraclePriceTooStale,
    
    #[msg("Insufficient gas for execution")]
    InsufficientGas,
    
    #[msg("Transaction too large")]
    TransactionTooLarge,
    
    #[msg("Invalid signature")]
    InvalidSignature,
    
    #[msg("Rate limit exceeded")]
    RateLimitExceeded,
    
    #[msg("Emergency shutdown active")]
    EmergencyShutdown,
    
    #[msg("Feature not implemented")]
    FeatureNotImplemented,
    
    #[msg("Invalid configuration")]
    InvalidConfiguration,
    
    #[msg("Maintenance mode active")]
    MaintenanceMode,
    
    #[msg("Invalid timestamp")]
    InvalidTimestamp,
    
    #[msg("Nonce already used")]
    NonceAlreadyUsed,
    
    #[msg("Invalid account discriminator")]
    InvalidAccountDiscriminator,
    
    #[msg("Account already exists")]
    AccountAlreadyExists,
    
    #[msg("Account not found")]
    AccountNotFound,
    
    #[msg("Invalid program data")]
    InvalidProgramData,
    
    #[msg("Program upgrade required")]
    ProgramUpgradeRequired,
    
    #[msg("Invalid version")]
    InvalidVersion,
    
    #[msg("Deprecated instruction")]
    DeprecatedInstruction,
    
    #[msg("Invalid instruction data")]
    InvalidInstructionData,
    
    #[msg("Cross-program invocation failed")]
    CpiCallFailed,
    
    #[msg("Invalid signer")]
    InvalidSigner,
    
    #[msg("Account not writable")]
    AccountNotWritable,
    
    #[msg("Account not executable")]
    AccountNotExecutable,
    
    #[msg("Invalid account owner")]
    InvalidAccountOwner,
    
    #[msg("Account data too small")]
    AccountDataTooSmall,
    
    #[msg("Account data too large")]
    AccountDataTooLarge,
    
    #[msg("Invalid account rent exemption")]
    InvalidAccountRentExemption,
    
    #[msg("Custom error")]
    CustomError,
}

impl From<FlashLoanError> for anchor_lang::error::Error {
    fn from(error: FlashLoanError) -> Self {
        anchor_lang::error::Error::from(anchor_lang::error::AnchorError {
            error_name: error.name(),
            error_code_number: error.into(),
            error_msg: error.to_string(),
            error_origin: Some(anchor_lang::error::ErrorOrigin::Source(anchor_lang::error::Source {
                filename: "error.rs",
                line: 0u32,
            })),
            compared_values: None,
        })
    }
}
```