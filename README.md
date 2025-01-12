# sBTC Options DEX Smart Contract

A sophisticated decentralized options trading platform for Bitcoin on Stacks that enables secure and efficient options trading for sBTC (Stacks Bitcoin).

## Overview

The sBTC Options DEX is a decentralized options trading platform implemented as a Clarity smart contract on the Stacks blockchain. It provides a secure and efficient way to create, trade, and exercise Bitcoin options using sBTC as the underlying asset.

## Features

- **Option Creation**: Create PUT and CALL options for sBTC with customizable parameters
- **Automated Trading**: Seamless premium calculations and settlements
- **Trustless Exercise**: Built-in mechanisms for option exercise without intermediaries
- **Risk Management**: Fully collateralized positions with comprehensive safety checks
- **Time-Locked Execution**: Enforced expiry and exercise windows
- **Access Controls**: Principal-based authorization system

## Core Functions

### Creating Options

```clarity
(create-option (sbtc-token <ft-trait>) (option-type (string-ascii 4)) (strike-price uint) (premium uint) (collateral uint) (expiry uint))
```

Creates a new option contract with the following parameters:

- `sbtc-token`: The sBTC fungible token contract
- `option-type`: Either "CALL" or "PUT"
- `strike-price`: The price at which the option can be exercised
- `premium`: The cost to purchase the option
- `collateral`: The amount of sBTC locked as collateral
- `expiry`: Block height at which the option expires

### Buying Options

```clarity
(buy-option (sbtc-token <ft-trait>) (option-id uint))
```

Allows users to purchase an existing option by:

- Transferring the premium to the option writer
- Becoming the new option holder
- Gaining the right to exercise the option

### Exercising Options

```clarity
(exercise-option (sbtc-token <ft-trait>) (option-id uint))
```

Enables option holders to exercise their options when:

- The option hasn't expired
- The current price conditions are favorable
- The option hasn't been previously exercised

### Expiring Options

```clarity
(expire-option (sbtc-token <ft-trait>) (option-id uint))
```

Allows option writers to reclaim their collateral after option expiry if:

- The option has reached its expiry block height
- The option hasn't been exercised
- The caller is the original option writer

## Security Features

### Collateralization

- All options must be fully collateralized at creation
- Collateral is locked in the contract until exercise or expiry
- Automatic collateral release mechanisms

### Time Constraints

- Minimum expiry period of 144 blocks (~1 day)
- Strict expiration enforcement
- Time-bound exercise windows

### Access Controls

- Writer-specific operations
- Holder-specific exercise rights
- Contract owner privileges

## Error Codes

| Code | Description          |
| ---- | -------------------- |
| u100 | Not authorized       |
| u101 | Invalid amount       |
| u102 | Option not found     |
| u103 | Option expired       |
| u104 | Insufficient balance |
| u105 | Invalid strike price |
| u106 | Invalid expiry       |
| u107 | Already exercised    |
| u108 | Invalid option type  |
| u109 | Zero amount          |
| u110 | Expiry too soon      |
| u111 | Not expired          |

## Constants

- `PRECISION`: 8 decimal places (100000000) for BTC amounts
- `MIN_EXPIRY_BLOCKS`: 144 blocks minimum expiry period
- Option Types: "CALL" and "PUT"

## State Management

### Data Maps

- `Options`: Stores all option contract details
- `UserBalances`: Tracks user balances

### Variables

- `next-option-id`: Unique identifier for new options
- `total-options-created`: Total number of options created
- `total-options-exercised`: Total number of exercised options

## Read-Only Functions

- `get-option`: Retrieve option details by ID
- `get-user-balance`: Get user's current balance
- `get-current-price`: Get current sBTC price
- `get-contract-stats`: Retrieve contract statistics

## Requirements

- Stacks blockchain network
- SIP-010 compliant sBTC token contract
- Clarity-compatible wallet for interactions

## Best Practices for Users

1. **Option Writers**

   - Ensure sufficient collateral before creating options
   - Monitor option expiry dates
   - Calculate premiums based on market conditions

2. **Option Buyers**

   - Verify option parameters before purchase
   - Track expiration dates
   - Understand exercise conditions

3. **General**
   - Always check current market prices before transactions
   - Verify gas fees and transaction costs
   - Understand the risks involved in options trading

## Technical Considerations

- All amounts use 8 decimal places precision
- Block heights are used for time calculations
- Price feeds should be consulted before exercising options
- Gas costs vary based on operation complexity

## License

This smart contract is open source and available under the MIT License.

## Security Considerations

- The contract implements principal-based access controls
- All mathematical operations use checked arithmetic
- Time-based operations use block heights for reliability
- Full collateralization prevents counterparty risk

## Contributing

Contributions are welcome! Please submit pull requests with:

- Clear description of changes
- Test cases for new functionality
- Documentation updates
-
