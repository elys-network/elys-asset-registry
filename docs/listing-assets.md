# Listing New Assets and Pools

The following guide outlines the steps to list a new IBC asset and liquidity pools on the Elys Network. This process is essential for projects that want to be integrated into the Elys ecosystem, ensuring they are available across all Elys-enabled applications and services.

This guide is structured to help you through the entire process, from creating a relayer to updating the asset registry. Each step includes detailed instructions and examples to ensure a smooth listing experience.

## 📚 Listing Steps

- [Pre requisites](#pre-requisites)
- [Creating a Relayer](#creating-a-relayer)
- [Adding the Asset Profile](#adding-the-asset-profile)
- [Adding the Asset Info](#adding-the-asset-info)
- [Reporting the Asset Price](#reporting-the-asset-price)
- [Creating an AMM Pool](#creating-an-amm-pool)
- [Enabling Rewards to the Pool](#enabling-rewards-to-the-pool)
- [Creating a Leverage LP Pool](#creating-a-leverage-lp-pool)
- [Creating a Vault](#creating-a-vault)
- [Enabling Rewards to the Vault](#enabling-rewards-to-the-vault)
- [Creating a Perpetual Pool](#creating-a-perpetual-pool)
- [Updating this Asset Registry](#updating-the-asset-registry)

## Pre-requisites

Before you begin, ensure you have the following:

- A project that is ready to be listed on the Elys Network.
- Git installed on your machine.
- A GitHub account to create pull requests in the Elys Network Asset Registry repository.
- Access to the Elys Network CLI. You can install it by following the [Elys CLI installation guide](https://github.com/elys-network/elys?tab=readme-ov-file#installation).

## Creating a Relayer

If you want to support a new chain, you will need to create a relayer to facilitate communication between the Elys Network and your chain. We recommend using [Hermes](https://hermes.informal.systems), which is widely supported and easy to set up. In case you don't have the required skills or resources to set up a relayer, please [reach out](https://discord.com/channels/1049783263956324462/1128804109718388906) to the Elys Network team for assistance.

You can ommit this step if you are listing an asset on a chain that already has a relayer set up.

## Adding the Asset Profile

The asset profile, contains general information about the asset, such as its name, symbol, and description. This profile is essential for identifying the asset within the Elys Network.

Execute the following transaction to add a new asset profile. Make sure to replace the placeholders with the actual values for your asset.

```bash
elysd tx assetprofile add-entry [base-denom] [decimals] [denom] [path] [ibc-channel-id] [ibc-counterparty-channel-id] [display-name] [display-symbol] [network] [address] [external-symbol] [transfer-limit] [permissions] [unit-denom] [ibc-counterparty-denom] [ibc-counterparty-chain-id] [commit-enabled] [withdraw-enabled] [flags]
```

Where:

- `base-denom`: The base denomination of the asset. i.e `uatom`.
- `decimals`: The number of decimal places for the asset. i.e `6`.
- `denom`: The IBC denomination of the asset. i.e `ibc/C4CFF46FD6DE35CA4CF4CE031E643C8FDC9BA4B99AE598E9B0ED98FE3A2319F9`.
- `path`: The IBC transfer path for the asset. i.e `transfer/channel-1`.
- `ibc-channel-id`: The IBC channel ID for the asset. i.e `channel-1`.
- `ibc-counterparty-channel-id`: The IBC counterparty channel ID for the asset. i.e `channel-1266`.
- `display-name`: The display name of the asset. i.e `ATOM`.
- `display-symbol`: The display symbol of the asset. i.e `uatom`.
- `network`: The network the asset belongs to. i.e `cosmoshub-4`.
- `address`: The address of the asset on the Elys Network. This is the same as the `authority` address: elys10d07y265gmmuvt4z0w9aw880jnsr700j6z2zm3.
- `external-symbol`: The external symbol of the asset. This is used for display purposes in applications. i.e `uatom`.
- `transfer-limit`: The transfer limit for the asset. This is optional and can be set to `0` for no limit.
- `permissions`: The permissions for the asset. This is optional and can be set to `[]` for no permissions.
- `unit-denom`: The unit denomination of the asset. This is typically the same as the `base-denom`. i.e `uatom`.
- `ibc-counterparty-denom`: The IBC counterparty denomination of the asset. This is typically the same as the `base-denom`. i.e `uatom`.
- `ibc-counterparty-chain-id`: The IBC counterparty chain ID for the asset. This is typically the same as the `network`. i.e `cosmoshub-4`.
- `commit-enabled`: Whether the asset is commit enabled. This is optional and can be set to `true` or `false`. i.e `true`.
- `withdraw-enabled`: Whether the asset is withdraw enabled. This is optional and can be set to `true` or `false`. i.e `true`.

## Adding the Asset Info

The asset info contains additional details about the asset, such as its ticker symbols and decimal places. This information is used by oracles and other services to provide accurate pricing data about the asset.

Execute the following transaction to add a new asset info. Make sure to replace the placeholders with the actual values for your asset.

```bash
elysd tx oracle create-asset-info [denom] [display] [band-ticker] [elys-ticker] [decimal] [flags]
```

Where:

- `denom`: The IBC denomination of the asset. i.e `ibc/C4CFF46FD6DE35CA4CF4CE031E643C8FDC9BA4B99AE598E9B0ED98FE3A2319F9`.
- `display`: The display name of the asset. i.e `ATOM`.
- `band-ticker`: The ticker symbol for the asset on Band Protocol. This is used for price feeds. i.e `ATOM`.
- `elys-ticker`: The ticker symbol for the asset on the Elys Network. This is used for display purposes in applications. i.e `ATOM`.
- `decimal`: The number of decimal places for the asset. i.e `6`.

## Reporting the Asset Price

<To be  completed>

## Creating an AMM Pool

AMM pools are the foundation of the Elys Network's liquidity infrastructure. They allow users to trade assets in a decentralized manner, providing liquidity and enabling further features like leverage LP pools, vaults and perpetuals.

All pools must be paired with Noble USDC: `ibc/F082B65C88E4B6D5EF1DB243CDA1D331D002759E938A0F5CD3FFDC5D53B3E349`.

Execute the following transaction to create a new AMM pool. Make sure to replace the placeholders with the actual values for your asset.

```bash
elysd tx amm create-pool [weights] [initial-deposit] [fee-denom] [flags]
```

Where:

- `weights`: The desired ratio of the assets in the pool. This is a comma-separated list of asset weights, usually . i.e `100ibc/622E4D024777290E25ACFB32CE149BB21EF785D78A0A3F297A7AF29A88325AED,100ibc/F082B65C88E4B6D5EF1DB243CDA1D331D002759E938A0F5CD3FFDC5D53B3E349`.
- `initial-deposit`: The initial deposit for the pool, which is the amount of each asset being added to the pool. This is a comma-separated list of amounts for each asset in the same order as the weights. The amounts should be wheited in the ratio previously specified. i.e `5000000000000000000ibc/622E4D024777290E25ACFB32CE149BB21EF785D78A0A3F297A7AF29A88325AED,3690000ibc/F082B65C88E4B6D5EF1DB243CDA1D331D002759E938A0F5CD3FFDC5D53B3E349`.
- `fee-denom`: The denomination of the fee asset. This is the asset that will be used to pay the swap fees for the pool. Typycally, USDC. i.e `ibc/F082B65C88E4B6D5EF1DB243CDA1D331D002759E938A0F5CD3FFDC5D53B3E349`.

Please copy the pool id from the transaction output after executing the command. You will need it for the next steps.

## Enabling Rewards to the AMM Pool

To enable `EDEN` rewards for the AMM pool, you need to send the following gov proposal to the Elys Network.

```proto
message MsgTogglePoolEdenRewards {
  option (cosmos.msg.v1.signer) = "authority";
  option (amino.name) = "masterchef/MsgTogglePoolEdenRewards";
  string authority = 1 [ (cosmos_proto.scalar) = "cosmos.AddressString" ];
  uint64 pool_id = 2;
  bool enable = 3;
}
```

- `authority`: The address of the gov authority: `elys10d07y265gmmuvt4z0w9aw880jnsr700j6z2zm3`
- `pool_id`: The ID of the AMM pool from the previous step. i.e `1`.
- `enable`: Whether to enable or disable rewards for the pool. i.e `true`.

To enable pool multipliers, you need to send the following gov proposal to the Elys Network.

```proto
message MsgUpdatePoolMultipliers {
  option (cosmos.msg.v1.signer) = "authority";
  option (amino.name) = "masterchef/MsgUpdatePoolMultipliers";
  string authority = 1 [ (cosmos_proto.scalar) = "cosmos.AddressString" ];
  repeated PoolMultiplier pool_multipliers = 2 [ (gogoproto.nullable) = false ];
}
```

Where:

- `authority`: The address of the gov authority: `elys10d07y265gmmuvt4z0w9aw880jnsr700j6z2zm3`
- `pool_multipliers`: A list of pool multipliers to be applied to the pool. Each multiplier is defined as follows:

```proto
message PoolMultiplier {
  uint64 pool_id = 1;
  string multiplier = 2 [
    (cosmos_proto.scalar) = "cosmos.Dec",
    (gogoproto.customtype) = "cosmossdk.io/math.LegacyDec",
    (gogoproto.nullable) = false
  ];
}
```

## Creating a Leverage LP Pool

To enable the leverage LP pool, you need to send the following gov proposal to the Elys Network.

```proto
message MsgAddPool {
  option (cosmos.msg.v1.signer) = "authority";
  option (amino.name) = "leveragelp/MsgAddPool";
  string authority = 1 [ (cosmos_proto.scalar) = "cosmos.AddressString" ];
  AddPool pool = 2 [ (gogoproto.nullable) = false ];
}
```

Where:

- `authority`: The address of the gov authority: `elys10d07y265gmmuvt4z0w9aw880jnsr700j6z2zm3`
- `pool`: The pool to be added, defined as follows:

```proto
message AddPool {
  uint64 amm_pool_id = 1; // The ID of the AMM pool to be used for the leverage LP pool.
  string pool_max_leverage_ratio = 2 [ // The maximum leverage ratio for the pool.
    (cosmos_proto.scalar) = "cosmos.Dec",
    (gogoproto.customtype) = "cosmossdk.io/math.LegacyDec",
    (gogoproto.nullable) = false
  ];
  string leverage_max = 3 [
    (cosmos_proto.scalar) = "cosmos.Dec",
    (gogoproto.customtype) = "cosmossdk.io/math.LegacyDec",
    (gogoproto.nullable) = false
  ];
}
```

## Creating a Vault

By depositing assets into a vault, users can earn interest on their holdings by lending them to leverage LP pools. This process is facilitated by the Elys Network's Stablestake module, which manages the vaults and interest rates.

To create a vault, please execute the following transaction:

```bash
elysd tx stablestake add-pool [deposit-denom] [interest-rate] [interest-rate-max] [interest-rate-min] [interest-rate-increase] [interest-rate-decrease] [health-factor] [max-leverage-ratio] [max-withdraw-ratio] [flags]
```

Where:

- `deposit-denom`: The IBC denomination of the asset to be deposited in the vault. i.e `ibc/694A6B26A43A2FBECCFFEAC022DEACB39578E54207FDD32005CD976B57B98004`.
- `interest-rate`: The initial interest rate for the vault.
- `interest-rate-max`: The maximum interest rate for the vault.
- `interest-rate-min`: The minimum interest rate for the vault.
- `interest-rate-increase`: The rate at which the interest rate increases.
- `interest-rate-decrease`: The rate at which the interest rate decreases.
- `health-factor`: The health factor for the vault.
- `max-leverage-ratio`: The maximum leverage ratio for the vault.
- `max-withdraw-ratio`: The maximum withdraw ratio for the vault.

## Enabling Rewards to the Vault

Just like AMM pools, vaults can also earn `EDEN` rewards. To enable rewards for the vault, you need to send the following gov proposal to the Elys Network.

```proto
message MsgTogglePoolEdenRewards {
  option (cosmos.msg.v1.signer) = "authority";
  option (amino.name) = "masterchef/MsgTogglePoolEdenRewards";
  string authority = 1 [ (cosmos_proto.scalar) = "cosmos.AddressString" ];
  uint64 pool_id = 2;
  bool enable = 3;
}
```

Where:

- `authority`: The address of the gov authority: `elys10d07y265gmmuvt4z0w9aw880jnsr700j6z2zm3`
- `pool_id`: The ID of the vault from the previous step. i.e `1
- `enable`: Whether to enable or disable rewards for the vault. i.e `true`.

## Creating a Perpetual Pool

Perpetual pools allow users to trade perpetual contracts on the Elys Network. These pools are designed to provide liquidity and enable leveraged trading.

To create a perpetual pool, you need to send the following gov proposal to the Elys Network.

```proto
message MsgUpdateEnabledPools {
  option (cosmos.msg.v1.signer) = "authority";
  option (amino.name) = "perpetual/MsgUpdateEnabledPools";
  string authority = 1 [ (cosmos_proto.scalar) = "cosmos.AddressString" ];
  repeated uint64 enabled_pools = 2;
  repeated uint64 add_pools = 3;
  repeated uint64 remove_pools = 4;
}
```

Where:

- `authority`: The address of the gov authority: `elys10d07y265gmmuvt4z0w9aw880jnsr700j6z2zm3`
- `enabled_pools`: A list of enabled perpetual pools. This is a list of pool IDs that are currently enabled for trading.
- `add_pools`: A list of pool IDs to be added to the enabled pools. You can use your AMM pool ID from the previous step.
- `remove_pools`: A list of pool IDs to be removed from the enabled pools.

## Updating the Asset Registry

Finally, you need to update the Elys Network Asset Registry to include the new asset and its associated pools. This is done by creating a pull request in the [Elys Network Asset Registry repository](https://github.com/elys-network/elys-asset-registry).

If you created the (Asset Registry)[#adding-the-asset-profile] entry for the new asset, there should be already an open pull request with the template to fill. Please check [Chain Schema](docs/SCHEMA.md) for the required fields and the expected format.

## Open questions

- assetprofile add-entry:
  - commit_enabled and withdraw_enabled: is this used? Otherwise, remove them.
  - permissions: is this used? Otherwise, remove it.
  - address: should we remove and keep authority address as the only address?
  - transfer-limit: is this used? Otherwise, remove it.
- assetinfo band-ticker or elys-ticker: is this being used?
- amm create-pool weights: should we remove this option and keep 100:100 as default?
- amm create-pool fee denom: never specified different than USDC. Should wee keep it as a required parameter?
- rewards: how pool multipliers should be treated?
- check out pool multipliers section. do we want users to execute that? What/how multiplier would be allowed/suggested?
- pool_max_leverage_ratio: how would we suggest/handle this value?
- vault creation: are we suggesting any values? users wont have context on how to set these values.
- MsgUpdateEnabledPools: should we refactor this so users dont have to send the whole list of enabled pools? Maybe we can have a `MsgAddEnabledPool` and `MsgRemoveEnabledPool` or just a toggle?
