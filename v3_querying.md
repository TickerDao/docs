# Querying TNS v3

## Overview

The Token Name Service v3 uses [The Graph](https://thegraph.com/) for efficient data querying. The endpoint for accessing data is `query.graph.tkn.xyz/subgraphs/name/tkn/v3`. Additionally, the contract data is stored on Data Chain and can be accessed through a proxy contract.

## GraphQL Queries

### Get Token Information by Symbol

Retrieve detailed information about a token using its symbol.

{% tabs %}
{% tab title="GraphQL" %}
```graphql
{
  tokens(where: { symbol: "WETH" }) {
    id
    name
    description
    symbol
    avatar
    dweb
    discord
    decimals
    addresses {
      tokenAddress      
      chainID {
        id
      }
    }
  }
}
```

This query returns:
- Basic token information (name, symbol, description)
- Media assets (avatar, dweb)
- Social links (discord)
- Technical details (decimals)
- Contract addresses across different chains
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const query = `{
  tokens(where: { symbol: "WETH" }) {
    id
    name
    description
    symbol
    avatar
    dweb
    discord
    decimals
    addresses {
      tokenAddress      
      chainID {
        id
      }
    }
  }
}`;

const response = await fetch('https://query.graph.tkn.xyz/subgraphs/name/tkn/v3', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({ query }),
});

const data = await response.json();
```
{% endtab %}
{% endtabs %}

### Get Token Information by Contract Address

Look up token information using a contract address.

{% tabs %}
{% tab title="GraphQL" %}
```graphql
query TokenByAddress {
  addresses(where: {tokenAddress: "0x5979D7b546E38E414F7E9822514be443A4800529"}) {
    addressID
    chainID {
      id
    }
    tokenAddress
    nonEVMAddress
    id
    tokenID {
      avatar
      description
      decimals
      name
      symbol
      tokenSupply
      twitter
    }
  }
}
```

This query returns:
- Chain information
- Token address details (EVM and non-EVM)
- Associated token metadata
- Social media links
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const query = `
  query TokenByAddress {
    addresses(where: {tokenAddress: "0x5979D7b546E38E414F7E9822514be443A4800529"}) {
      addressID
      chainID {
        id
      }
      tokenAddress
      nonEVMAddress
      id
      tokenID {
        avatar
        description
        decimals
        name
        symbol
        tokenSupply
        twitter
      }
    }
  }
`;

const response = await fetch('https://query.graph.tkn.xyz/subgraphs/name/tkn/v3', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({ query }),
});

const data = await response.json();
```
{% endtab %}
{% endtabs %}

## Contract Queries

The contract data is stored on Data Chain and can be accessed through the proxy contract at [0x7b02d739D95758F09dDc7e44FA951cd45D97247A](https://datachain-sepolia.explorer.caldera.xyz/address/0x7b02d739D95758F09dDc7e44FA951cd45D97247A?tab=read_proxy).

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const abi = [/* See full ABI in repository */];
const contractAddress = "0x7b02d739D95758F09dDc7e44FA951cd45D97247A";
const contract = new ethers.Contract(contractAddress, abi, provider);

// Get token ID by symbol
const tokenId = await contract.getTokenIdBySymbol("WETH");

// Get token data
const tokenName = await contract.getTokenData(tokenId, "name");
const tokenDescription = await contract.getTokenData(tokenId, "description");
const tokenDecimals = await contract.getTokenData(tokenId, "decimals");

// Get token address for a specific chain
const addressInfo = await contract.getAddressBySymbolAndChain("WETH", "1"); // chainId 1 for Ethereum mainnet
```
{% endtab %}

{% tab title="Web3.js" %}
```javascript
const contractAddress = "0x7b02d739D95758F09dDc7e44FA951cd45D97247A";
const contract = new web3.eth.Contract(abi, contractAddress);

// Get token ID by symbol
const tokenId = await contract.methods.getTokenIdBySymbol("WETH").call();

// Get token data
const tokenName = await contract.methods.getTokenData(tokenId, "name").call();
const tokenDescription = await contract.methods.getTokenData(tokenId, "description").call();
const tokenDecimals = await contract.methods.getTokenData(tokenId, "decimals").call();

// Get token address for a specific chain
const addressInfo = await contract.methods.getAddressBySymbolAndChain("WETH", "1").call();
```
{% endtab %}

{% tab title="Solidity" %}
```solidity
pragma solidity ^0.8.0;

interface ITNS {
    function getTokenIdBySymbol(string calldata symbol) external view returns (uint256);
    function getTokenData(uint256 tokenId, string calldata key) external view returns (string memory);
    function getAddressBySymbolAndChain(string calldata symbol, string calldata chainId) 
        external view returns (string memory tokenAddress, string memory nonEVMAddress);
}

contract TokenQuery {
    ITNS public tns;
    
    constructor(address _tnsAddress) {
        tns = ITNS(_tnsAddress);
    }
    
    function getTokenInfo(string calldata symbol) external view returns (
        uint256 tokenId,
        string memory name,
        string memory description,
        string memory decimals
    ) {
        tokenId = tns.getTokenIdBySymbol(symbol);
        name = tns.getTokenData(tokenId, "name");
        description = tns.getTokenData(tokenId, "description");
        decimals = tns.getTokenData(tokenId, "decimals");
    }
    
    function getTokenAddress(string calldata symbol, string calldata chainId) external view returns (
        string memory tokenAddress,
        string memory nonEVMAddress
    ) {
        return tns.getAddressBySymbolAndChain(symbol, chainId);
    }
}
```
{% endtab %}
{% endtabs %}

## Additional Resources

- [TNS v3 Graph Explorer](https://query.graph.tkn.xyz/subgraphs/name/tkn/v3)
- [Data Chain Contract](https://datachain-sepolia.explorer.caldera.xyz/address/0x7b02d739D95758F09dDc7e44FA951cd45D97247A?tab=read_proxy)