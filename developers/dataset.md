---
description: A description of the metadata maintained by TNS for each token
---

# Dataset

Below are detailed descriptions and rules for each item in the TNS dataset, including `name, description, avatar, url, contractAddress, decimals, twitter, github, dweb, version,` and various sidechain contract addresses.

See the following page for information on querying the TNS dataset.

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

## `notice`

Notification and context around token data changes\
**Example:** `LEND is migrating to AAVE, at a rate of 100 LEND to 1 AAVE. The migration interface can be viewed at` [`https://app-v1.aave.com/migration-portal`](https://app-v1.aave.com/migration-portal)

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

## `version`

A monotonically increasing versioning identifier, that increases when a token's metadata is updated.&#x20;

X.0.0 is increased when a ticker ($XXX) is changed, \
0.X.0 is increased when a contract address is changed, \
0.0.X is increased when token metadata is updated.&#x20;

If a more significant number is changed, all the numbers under it are reset. The starting version is 0.0.1

**Example:** 0.1.3 - The first version of a token, on its second token contract address, under which, the metadata has been updated twice.

## Sidechain Addresses

The token's contract addresses for EVM and non-EVM chains.&#x20;

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

Goerli Testnet | goerli`_address`

Sepolia Testnet | sepolia`_address`

### Non-EVM Networks:

Networks that don't conform to the EVM standard

NEAR | `near_address`\
**Example:** 2260fac5e5542a773aa44fbcfedf7c193bc2c599.factory.bridge.near

Solana | `sol_address`\
**Example:** 3NZ9JMVBmGAqocybic2c7LQCJScmgsAZ6vQqTDzcqmJh

Tron | `trx_address`\
**Example:** TXpw8XeWYeTUd4quDskoUqeQPowRh4jY65

Zilliqa | `zil_address`\
**Example:** zil1wha8mzaxhm22dpm5cav2tepuldnr8kwkvmqtjq

## Upcoming Datapoints:

Future datapoints that will be added to the dataset. Additional datapoints can be authorized through a governance vote.

## `tokenSupply`

The total number of tokens created for a project\
**Example:** 10000000

## `circulatingSupply`

The total number of tokens in circulation \
_This may be defined by a function in future versions of the resolver contract_\
**Example:** 7800000

## `chain_id`

The chain id, if the token represents an EVM network\
**Example:** 10

## `discord`

The invite string for a project's discord server\
**Example:** [dEc9GHjKUP](https://discord.com/invite/dEc9GHjKUP)

## `forum`

The URL for a project's forum\
**Example:** [discuss.ens.domains](https://discuss.ens.domains/)

## `governance`

The URL for the Tally.xyz or other onchain voting interface\
**Example:** [tally.xyz/gov/ens](https://www.tally.xyz/gov/ens)

## `snapshot`

The URL for the Snapshot.org or other off-chain voting interface\
**Example:** [snapshot.org/#/ens.eth](https://snapshot.org/#/ens.eth)

## `abi`

The ABI interface for the token contract

## `git`

Host agnostic source code url (Gitlab, Bitbucket, Radicle, ect)

## `governance`

The contract address for the governance contract\
**Example:** 0x690e775361AD66D1c4A25d89da9fCd639F5198eD
