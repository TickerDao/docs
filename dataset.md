---
description: A description of the token metadata maintained by TNS
---

# Dataset

## Token Metadata

The data types maintained by TNS that can be queried for each token&#x20;

## `name`

A short string containing the name of the project\
**Example:** `Wrapped Bitcoin`&#x20;

## `description`

A succinct, one sentence description of the project\
**Example:** `Wrapped bitcoin token with BTC held in reserve`

## `avatar`

A public URL linking to the project's logo image, hosted on IPFS. Developers can use the logo URL without needing to be familiar with [IPFS](https://ipfs.tech/). Decentralization oriented developers can parse the IPFS [CID](https://docs.ipfs.tech/concepts/content-addressing/) from the end of the URL.\
**Example:** [https://gateway.tkn.xyz/ipfs/bafybeibewoge5bpwedf6nmd4c6oospxxpufrnrwdkcubomn267ivjd2lam](https://gateway.tkn.xyz/ipfs/bafybeibewoge5bpwedf6nmd4c6oospxxpufrnrwdkcubomn267ivjd2lam)

## `url`

The URL of the main website used by the project\
**Example:** [https://wbtc.network/](https://wbtc.network/)

## `contractAddress`

The hex address of the token contract on Ethereum mainnet. Tokens may reside on other chains, or have their own blockspace, in which case this field may be blank. Tokens may have multiple addresses on multiple chains. If so, see sidechain address fields below.\
**Example:** 0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599

## **`decimals`**

The number of decimals that the token can be divided by. A requirement of the [erc20 token standard](https://docs.openzeppelin.com/contracts/3.x/api/token/erc20#ERC20-decimals--).\
**Example:** 18

## `twitter`

The twitter handle used by the project, without the '@'. Capitalization is permitted.\
**Example:** WrappedBTC

## `github`

The name of the GitHub organization used by the project\
**Example:** WrappedBTC

## `dweb`

A decentralized IPFS, IPNS, Swarm, or Arweave hash, used by browsers to resolve web pages with decentralized hosting.\
**Example:** bafybeiaqs3f3wcfln2ieej2wk6emuqcnc6rixahiv4ruqvjlmxlrkhaqdq

## Sidechain Addresses

Token contract addresses for EVM and non-EVM chains.&#x20;

### EVM Networks:

Networks with contract addresses that conform to the EVM standard\
**Example:** 0x68f180fcCe6836688e9084f035309E29Bf0A2095

_Current networks include:_

Optimism | `op_address`

Arbirum | `arb1_address`

Avalanche (C-Chain) | `avaxc_address`

Binance Smart Chain | `bsc_address`

Cronos | `cro_address`

Fantom | `ftm_address`

Gnosis Chain | `gno_address`

Polygon | `matic_address`

### Non-EVM Networks:

Networks that don't conform to the EVM standard

NEAR | `near_address`\
**Example:** 2260fac5e5542a773aa44fbcfedf7c193bc2c599.factory.bridge.near

Solana | `sol_address`\
**Example:** 3NZ9JMVBmGAqocybic2c7LQCJScmgsAZ6vQqTDzcqmJh

Tron | `trx_address`\
**Example:** TXpw8XeWYeTUd4quDskoUqeQPowRh4jY65

Ziliqa | `zil_address`\
**Example:** zil1wha8mzaxhm22dpm5cav2tepuldnr8kwkvmqtjq



``