# Listing New Assets and Pools

The following guide outlines the steps to list new assets and liquidity pools on the Elys Network. This process is essential for projects that want to be integrated into the Elys ecosystem, ensuring they are available across all Elys-enabled applications and services.

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
- [Enabling Rewards to the Leverage Pool](#enabling-rewards-to-the-leverage-pool)
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

## Enabling Rewards to the Pool

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

Where:

- `authority`: The address of the gov authority: `elys10d07y265gmmuvt4z0w9aw880jnsr700j6z2zm3`
- `pool_id`: The ID of the AMM pool from the previous step. i.e `1`.
- `enable`: Whether to enable or disable rewards for the pool. i.e `true`.

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
