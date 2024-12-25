# distribute-income
Scripts to calculate, normalize, and distribute income to stakers. You can use our other repos to implement staking.

[On GitHub](https://github.com/bark-ruffalo), we've open-sourced our **contracts, our scripts, and the UI (website)**. Any other crypto or AI agent project may use them; we just ask you to consider airdropping our DAO address (`0xc638FB83d2bad5dD73d4C7c7deC0445d46a0716F`) or our stakers (we can provide a list of addresses, or you can use `getLockedUsersByPool()` on [our staking contract](https://basescan.org/address/0xA6FaCD417faf801107bF19F4a24062Ff15AE9C61#readContract)). We'll help you get started if you need help with the code (@nebu_human and @BatataKawaii on Telegram, or @TrulyADog on X).


## Setup

1. Install dependencies:
```bash
npm install
```

2. Copy `.env.example` to `.env` and fill in your values:
```bash
cp .env.example .env
```

## Scripts

### getHighStakers.js

This script analyzes staking positions and generates a detailed report of high-value stakers, including their stakes across different pools and adjusted values.

#### Features
- Identifies stakers above 5000 tokens threshold
- Applies pool-specific modifiers:
  - Pool 0 ($PAWSY): 1x (base value)
  - Pool 1 ($mPAWSY): 1.1x (10% bonus)
  - Pool 2 (LP): 93.69x (LP conversion rate)
- Generates detailed breakdown of stakes and adjusted values
- Saves results to a timestamped JSON file

#### Command Line Arguments
- `--nocalc`: Skip income distribution calculations
- `--simulate=amount1,amount2,...`: Add simulated stakers with specified amounts

Example:
```bash
node getHighStakers.js
node getHighStakers.js --nocalc
node getHighStakers.js --simulate=10000,20000
```

### distributeIncome.js

This script calculates and executes token distribution to stakers based on their adjusted stakes.

#### Features
- Reads staking snapshot from JSON file
- Supports stake normalization (reducing extreme values)
- Calculates token distribution
- Can execute actual token transfers

#### Command Line Arguments
- `--normalize`: Apply normalization to reduce extreme values (20% towards mean)
- `--doit`: Execute actual token transfers (requires private key in .env)

Example:
```bash
node distributeIncome.js --normalize
node distributeIncome.js --normalize --doit
```

## Configuration

The scripts use environment variables for configuration:
- `BASE_RPC_URL`: RPC endpoint for Base network
- `DEPLOYER_PRIVATE_KEY`: Private key for executing transfers (only needed with --doit flag)

Contract addresses are configured in the scripts:
- Staking Vault: `0xA6FaCD417faf801107bF19F4a24062Ff15AE9C61`
- Token: `0x29e39327b5B1E500B87FC0fcAe3856CD8F96eD2a`
