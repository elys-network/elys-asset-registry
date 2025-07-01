# Chain Asset Schema Documentation

This document explains the structure and requirements for chain asset files in the Elys Asset Registry.

## 📋 Overview

Each chain asset file contains configuration for a single blockchain network, including RPC endpoints, explorer URLs, IBC channels, and supported currencies with their capabilities.

## 📊 Data Structure

```json
{
  "chains": {
    "elys": {
      "chainId": "elys-1",
      "chainName": "Elys",
      "addressPrefix": "elys",
      "rpcURL": "https://rpc.elys.network:443",
      "restURL": "https://api.elys.network:443",
      "explorerURL": {
        "transaction": "https://mainnet.itrocket.net/elys/tx/{transaction}"
      },
      "channel": {
        "source": "",
        "destination": ""
      },
      "isEnabled": true,
      "priority": 1,
      "currencies": [
        {
          "coinDenom": "ELYS",
          "coinMinimalDenom": "uelys",
          "coinIbcDenom": "",
          "coinDecimals": 6,
          "coinGeckoId": "elys",
          "coinImageUrl": "/tokens/elys.svg",
          "isFeeCurrency": true,
          "isStakeCurrency": false,
          "canSwap": true,
          "canWithdraw": true,
          "canDeposit": true,
          "canUseLiquidityMining": true,
          "canUseLeverageLP": true,
          "canUsePerpetual": false,
          "canUseVaults": true,
          "gasPriceStep": {
            "low": 0.01,
            "average": 0.025,
            "high": 0.03
          }
        }
      ]
    }
  }
}
```

## 🏗️ Root Structure

```json
{
  "chainId": "string",
  "chainName": "string",
  "addressPrefix": "string",
  "rpcURL": "string",
  "restURL": "string",
  "explorerURL": { ... },
  "channel": { ... },
  "currencies": [ ... ]
}
```

## 🔧 Chain Properties

### `chainId` (required)

- **Type**: `string`
- **Description**: Unique identifier for the blockchain network
- **Example**: `"elys-1"`, `"cosmoshub-4"`, `"osmosis-1"`
- **Validation**: Must be at least 1 character long

### `chainName` (required)

- **Type**: `string`
- **Description**: Human-readable name of the blockchain
- **Example**: `"Elys"`, `"Cosmos Hub"`, `"Osmosis"`
- **Validation**: Must be at least 1 character long

### `addressPrefix` (required)

- **Type**: `string`
- **Description**: Address prefix used for bech32 addresses, or address format identifier
- **Example**: `"elys"`, `"cosmos"`, `"osmo"`, `"0x"` (for Ethereum)
- **Validation**: Must be at least 1 character long

### `rpcURL` (required)

- **Type**: `string`
- **Description**: RPC endpoint URL with port number
- **Example**: `"https://rpc.elys.network:443"`
- **Validation**: Must match pattern `https?://[domain]:[port]`

### `restURL` (required)

- **Type**: `string`
- **Description**: REST API endpoint URL with port number
- **Example**: `"https://api.elys.network:443"`
- **Validation**: Must match pattern `https?://[domain]:[port]`

## 🔍 Explorer URLs

The `explorerURL` object contains URL templates for blockchain explorers:

```json
{
  "explorerURL": {
    "transaction": "https://explorer.com/tx/{transaction}",
    "account": "https://explorer.com/account/{account}",
    "validator": "https://explorer.com/validator/{validator}",
    "block": "https://explorer.com/block/{block}"
  }
}
```

### Properties

- **`transaction`** (required): URL template for transaction pages
- **`account`** (optional): URL template for account pages
- **`validator`** (optional): URL template for validator pages
- **`block`** (optional): URL template for block pages

### URL Templates

Use placeholders in curly braces that will be replaced with actual values:

- `{transaction}` - Transaction hash
- `{account}` - Account address
- `{validator}` - Validator address
- `{block}` - Block height or hash

## 🌉 IBC Channels

The `channel` object specifies IBC channel configuration:

```json
{
  "channel": {
    "source": "channel-1",
    "destination": "channel-1266"
  }
}
```

### Properties

- **`source`** (required): Source (Elys) IBC channel identifier
- **`destination`** (required): Destination chain IBC channel identifier
- **Format**: `"channel-{number}"` or empty string `""`
- **Example**: `"channel-1"`, `"channel-42"`, `""`

## 💰 Currencies

The `currencies` array contains all supported tokens/currencies on the chain:

```json
{
  "currencies": [
    {
      "coinDenom": "ELYS",
      "coinDisplayDenom": "Elys",
      "coinMinimalDenom": "uelys",
      "coinIbcDenom": "",
      "coinDecimals": 6,
      "coinGeckoId": "elys",
      "canSwap": true,
      "isFeeCurrency": true,
      "isStakeCurrency": true,
      "canWithdraw": true,
      "canDeposit": true,
      "canUseLiquidityMining": true,
      "canUseLeverageLP": false,
      "canUsePerpetual": false,
      "canUseVaults": true,
      "gasPriceStep": { ... },
      "cc": { ... }
    }
  ]
}
```

## 🪙 Currency Properties

### Basic Information

#### `coinDenom` (required)

- **Type**: `string`
- **Description**: Trading symbol/ticker (used as base in price APIs and as lockup key)
- **Example**: `"ELYS"`, `"ATOM"`, `"OSMO"`
- **Validation**: Only uppercase letters and numbers, 1-10 characters
- **Pattern**: `^[A-Z0-9]+$`

#### `coinDisplayDenom` (required)

- **Type**: `string`
- **Description**: In case the display name differs from `coinDenom`, this is used for user interfaces. Otherwise, keep it the same.
- **Example**: `"Elys"`, `"Cosmos"`, `"USD Coin"`

#### `coinMinimalDenom` (required)

- **Type**: `string`
- **Description**: Smallest unit denomination used on-chain. Must be the same than the one used in the destination chain.
- **Example**: `"uelys"` (micro-elys), `"uatom"` (micro-atom)

#### `coinIbcDenom` (required)

- **Type**: `string`
- **Description**: Elys IBC denomination hash of the asset.
- **Example**: `"ibc/C4CFF46FD6DE35CA4CF4CE031E643C8FDC9BA4B99AE598E9B0ED98FE3A2319F9"`
- **Note**: Can be empty `""` for native tokens
- **Validation**: Must match IBC hash format or be empty

#### `coinDecimals` (required)

- **Type**: `integer`
- **Description**: Number of decimal places for parsing and formatting.
- **Example**: `6` (1 ELYS = 1,000,000 uelys), `18` (for Ethereum tokens)
- **Range**: 0-18

#### `coinGeckoId` (required)

- **Type**: `string`
- **Description**: CoinGecko API identifier for price data
- **Example**: `"elys"`, `"cosmos"`, `"usd-coin"`
- **Note**: Can be empty `""` if not listed on CoinGecko
- **Validation**: Lowercase letters, numbers, and hyphens only

### Capability Flags

These boolean flags indicate what features the currency supports:

#### `canSwap` (required)

- **Type**: `boolean`
- **Description**: Enables swaps on the frontend apps
- **Validation**: There must be one amm pool already created on chain

#### `isFeeCurrency` (required)

- **Type**: `boolean`
- **Description**: Whether the currency can be used to pay transaction fees on the Elys chain
- **Validation**: Validators must support this currency for fees

#### `isStakeCurrency` (required)

- **Type**: `boolean`
- **Description**: Whether the currency can be staked for network security rewards

#### `canWithdraw` (required)

- **Type**: `boolean`
- **Description**: Enables withdraws on the frontend apps
- **Validation**: There must an IBC enabled channel for this currency

#### `canDeposit` (required)

- **Type**: `boolean`
- **Description**: Enables deposits on the frontend apps
- **Validation**: There must an IBC enabled channel for this currency

#### `canUseLiquidityMining` (required)

- **Type**: `boolean`
- **Description**: Enables liquidity mining pool on the frontend apps.
- **Validation**: There must be one amm pool already created on chain

#### `canUseLeverageLP` (required)

- **Type**: `boolean`
- **Description**: Enables leverage LP pool on the frontend apps.
- **Validation**: There must be one amm pool already created on chain and leverage LP must be enabled through governance.

#### `canUsePerpetual` (required)

- **Type**: `boolean`
- **Description**: Enables perpetual trading on the frontend apps.
- **Validation**: There must be one amm pool already created on chain and leverage LP must be enabled through governance.

#### `canUseVaults` (required)

- **Type**: `boolean`
- **Description**: Enables vaults on the frontend apps.
- **Validation**: There must be one amm pool already created on chain, leverage LP must be enabled through governance and vault must exist on chain.

## ⛽ Gas Price Configuration

The `gasPriceStep` object defines gas price levels:

```json
{
  "gasPriceStep": {
    "low": 0.01,
    "average": 0.025,
    "high": 0.03
  }
}
```

### Properties

- **`low`** (required): Low gas price for slower transactions
- **`average`** (required): Standard gas price for normal speed
- **`high`** (required): High gas price for faster transactions
- **Type**: `number`
- **Validation**: Must be >= 0

## 📊 CryptoCompare Configuration

The `cc` object configures price data fetching:

```json
{
  "cc": {
    "quote": "USDT",
    "exchange": "binance"
  }
}
```

### Properties

#### `quote` (required)

- **Type**: `string`
- **Description**: Quote currency for price pairs
- **Example**: `"USDT"`, `"USD"`, `"BTC"`
- **Validation**: Uppercase letters only, 1-10 characters

#### `exchange` (required)

- **Type**: `string`
- **Description**: Exchange platform for price data
- **Allowed values**:
  - `"binance"`
  - `"coinbase"`
  - `"kraken"`
  - `"gateio"`
  - `"mexc"`
  - `"kucoin"`
  - `"huobi"`
  - `"okx"`
  - `"bitfinex"`
  - `"bybit"`

### Usage

The CryptoCompare configuration creates price pairs using:

- **Base**: `coinDenom` value (automatically used)
- **Quote**: `cc.quote` value
- **Exchange**: `cc.exchange` value

Example: For ELYS with `"quote": "USDT"` and `"exchange": "binance"`, the price pair becomes `ELYS/USDT` on Binance.

## 📝 Complete Example

```json
{
  "chainId": "elys-1",
  "chainName": "Elys Network",
  "addressPrefix": "elys",
  "rpcURL": "https://rpc.elys.network:443",
  "restURL": "https://api.elys.network:443",
  "explorerURL": {
    "transaction": "https://ping.pub/elys/tx/{transaction}",
    "account": "https://ping.pub/elys/account/{account}",
    "validator": "https://ping.pub/elys/staking/{validator}"
  },
  "channel": {
    "source": "",
    "destination": ""
  },
  "currencies": [
    {
      "coinDenom": "ELYS",
      "coinDisplayDenom": "Elys",
      "coinMinimalDenom": "uelys",
      "coinIbcDenom": "",
      "coinDecimals": 6,
      "coinGeckoId": "elys",
      "canSwap": true,
      "isFeeCurrency": true,
      "isStakeCurrency": true,
      "canWithdraw": true,
      "canDeposit": true,
      "canUseLiquidityMining": true,
      "canUseLeverageLP": false,
      "canUsePerpetual": true,
      "canUseVaults": true,
      "gasPriceStep": {
        "low": 0.01,
        "average": 0.025,
        "high": 0.05
      },
      "cc": {
        "quote": "USDT",
        "exchange": "gateio"
      }
    }
  ]
}
```

## ✅ Validation Rules

### Required Fields

All fields marked as `(required)` must be present and non-empty.

### Data Types

- Strings must meet minimum length requirements
- Numbers must be >= 0 where specified
- Booleans must be `true` or `false`

### Format Validation

- URLs must include protocol and port
- Chain IDs should follow standard format
- IBC denominations must match expected patterns
- Exchange names must be from allowed list

### Business Rules

- At least one currency must be defined
- Gas price steps should be in ascending order (low < average < high)
- IBC channels should follow `channel-{number}` format when not empty

The CI pipeline automatically validates all chain files on every commit and pull request.
