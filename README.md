# Token Name Service Docs

The Basics

The Token Name Service uses ENS, and can be queried anywhere .eth domains are supported

The Token name set is persisted as subdomains of the `*.tkn.eth` domain

For instance, the contract for USDC can be found at `usdc.tkn.eth` and Wrapped Bitcoin at `wbtc.tkn.eth`

Descriptive metadata can be found on chain in each token's ENS resolver.
Including `name, description, avatar, url, decimals, twitter, & github`.

The TNS.sol contract exposes additional queries; such as: fetch all metadata, and token balance for address. Ticker.sol is hosted at `tkn.eth`
