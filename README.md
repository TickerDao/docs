---
description: Overview of TokenDAO
---

# Introduction

TokenDAO helps the ecosystem define a shared `token` data type and facilitates the creation of a shared dataset containing important information about each token. Token data stored in TokenDAO can be integrated in a permissionless manner and used in any context. Wallets, web apps, smart contracts, token communities can now exercise direct control over the updates they wish to see in this shared token dataset, define open and transparent update processes, set validity durations on data to maintain this shared dataset.

Tickers are unique and short symbols set by TokenDAO within the shared dataset for each token community. TokenDAO believes that communities should naturally own their symbols together unlike an ENS name which has to be owned. Even shared ownership of ENS names with multi-sig wallets is not fool-proof. Subscription fees, domain renewals, expiry dates to allow others to grab your domain name should not apply to important symbols like Tickers.

Tickers are unique, hard to mix up, less prone to typos, easy to remember, and efficient to type. They allow safe and efficient access to information about a token in the shared token dataset.&#x20;

### Example&#x20;

UNI ticker within TokenDAO is assigned to the popular Uniswap community because it is naturally owned by them today. This allows different types of users operating within different contexts to use the UNI ticker symbol,&#x20;

* Smart Contract developer can fetch UNI token address on ethereum with tokendao.getValue(eth\_addr)
* User can access the latest published decentralized uniswap website hosted on ipfs using https://uni.tkn.eth.limo&#x20;
* Trader can access the popular dex application hosted on ipfs from https://dex.uni.tkn.eth.limo
* Wallets can lookup logo information using the UNI token symbol.
* Web apps can reverse lookup and show their users that they are interacting with trusted contract addresses like the latest Uniswap Router by showing router.uni.tkn.eth instead of the hexadecimal ethereum address
* Front end developers can integrate with the decentralized token list to allow users to search for the right UNI token.

### Unique Features

* TokenDAO hardens token data through clear governance and update procedures on the shared token dataset.
* TokenDAO makes it very easy to update. No more centralized gatekeepers to submit forms which go into the dark.
* TokenDAO is fully governed. Your community and users can breathe easy knowing your decentralized name will never expire or a malicious hacker can take control.&#x20;
* Data is served from the blockchain. Blockchains like Ethereum have 100% uptime, DDOS Proof, and are the best possible CDNs a data point can be served from!

### The Basics

The Token Name Service uses ENS, and can be queried anywhere .eth domains are supported

The Token name set is persisted as subdomains of the `*.tkn.eth` domain

For instance, the contract for USDC can be found at `usdc.tkn.eth` and Wrapped Bitcoin at `wbtc.tkn.eth`

Descriptive metadata can be found on chain in each token's ENS resolver. Including `name, description, avatar, url, decimals, twitter, & github`.

The TNS.sol contract exposes additional queries; such as: fetch all metadata, and token balance for address. Ticker.sol is hosted at `tkn.eth`
