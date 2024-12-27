# Stacks Bitcoin Trading Contract V2

## Overview

The Bitcoin Options Trading Contract V2 is a decentralized smart contract designed for peer-to-peer options trading with internal Bitcoin (BTC) balance management. It supports both CALL and PUT options, ensuring secure trading and transparent execution on the Stacks blockchain. This contract allows users to create, purchase, and exercise options, all while tracking BTC balances internally.

## Key Features
- **Decentralized Trading**: Peer-to-peer options trading without intermediaries.
- **CALL and PUT Options**: Support for both option types with customizable terms.
- **BTC Balance Management**: Internal BTC tracking for secure and transparent transactions.
- **Automated Execution**: Options can be exercised automatically based on predefined terms.
- **Role-based Authorization**: Only the contract owner can update key parameters like BTC prices.

## Data Structures

### **BTCBalances**
Tracks internal BTC balances for each user.
- **Keys**: `{ holder }`
- **Values**:
  - `balance`: Amount of BTC held.

### **Options**
Stores all created options with their details.
- **Keys**: `{ option-id }`
- **Values**:
  - `creator`: Address of the option creator.
  - `buyer`: (Optional) Address of the option buyer.
  - `option-type`: Type of option (`CALL` or `PUT`).
  - `strike-price`: Price at which the option can be exercised.
  - `premium`: Cost of the option.
  - `expiry`: Block height when the option expires.
  - `btc-amount`: Amount of BTC involved in the option.
  - `is-active`: Boolean indicating if the option is active.
  - `is-executed`: Boolean indicating if the option has been exercised.
  - `creation-height`: Block height when the option was created.

### **UserOptions**
Tracks options created and purchased by a user.
- **Keys**: `{ user }`
- **Values**:
  - `created`: List of option IDs created by the user.
  - `purchased`: List of option IDs purchased by the user.

### **Contract Variables**
- **`next-option-id`**: ID for the next option to be created.
- **`oracle-price`**: Current BTC price set by the contract owner.

## Public Functions

### **BTC Balance Management**

#### `deposit-btc`
Deposits BTC into the user’s internal balance.
- **Parameters**:
  - `amount`: Amount of BTC to deposit.
- **Returns**: `true` on success.

#### `withdraw-btc`
Withdraws BTC from the user’s internal balance.
- **Parameters**:
  - `amount`: Amount of BTC to withdraw.
- **Returns**: `true` on success.

### **Option Management**

#### `create-option`
Creates a new CALL or PUT option.
- **Parameters**:
  - `option-type`: Type of option (`CALL` or `PUT`).
  - `strike-price`: Exercise price of the option.
  - `premium`: Price of the option.
  - `expiry`: Expiry block height.
  - `btc-amount`: Amount of BTC involved.
- **Returns**: Option ID.

#### `exercise-option`
Executes the option if valid and within expiry.
- **Parameters**:
  - `option-id`: ID of the option to exercise.
- **Returns**: `true` on success.

### **Read-Only Functions**

#### `get-option`
Fetches details of a specific option.
- **Parameters**:
  - `option-id`: ID of the option.
- **Returns**: Option details.

#### `get-btc-balance`
Retrieves the BTC balance of a user.
- **Parameters**:
  - `holder`: Address of the user.
- **Returns**: BTC balance.

#### `get-user-options`
Gets options created and purchased by a user.
- **Parameters**:
  - `user`: Address of the user.
- **Returns**: Lists of created and purchased option IDs.

#### `get-btc-price`
Returns the current BTC price.
- **Returns**: Oracle price.

### **Oracle Functions**

#### `set-btc-price`
Updates the BTC price used for option calculations.
- **Parameters**:
  - `price`: New BTC price.
- **Returns**: `true` on success.

## Security and Validation

- **Authorization**:
  - Only the contract owner can update the BTC price.
- **Validation**:
  - Ensures sufficient balance for CALL options.
  - Validates option terms (e.g., positive strike price, premium, and BTC amount).
  - Prevents duplicate or expired operations.
- **Error Codes**:
  - `ERR-EXPIRED`: Option has expired.
  - `ERR-INVALID-AMOUNT`: Invalid BTC amount specified.
  - `ERR-UNAUTHORIZED`: Unauthorized action attempted.
  - `ERR-NOT-FOUND`: Option not found.
  - `ERR-INSUFFICIENT-BALANCE`: Insufficient BTC balance.
  - `ERR-ALREADY-EXECUTED`: Option has already been exercised.
  - `ERR-INVALID-PRICE`: Invalid strike price or premium specified.
  - `ERR-OPTION-NOT-ACTIVE`: Option is not active.

## Deployment
- Deploy the contract to the Stacks blockchain.
- Set initial values for `oracle-price` and `next-option-id`.

## Future Enhancements
- Integration with external BTC price oracles for automatic price updates.
- Support for partial option exercise.
- Enhanced reward mechanisms for option creators and buyers.
- Dynamic fee adjustments based on market conditions.

---

This contract empowers decentralized options trading with transparency, security, and ease of use. Engage in trading and manage your BTC balances seamlessly on-chain!

