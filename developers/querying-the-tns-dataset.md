# Querying the TNS Dataset

## Documentation

The Token Name Service uses [ENS](https://ens.domains/), and can be queried anywhere .eth domains are supported

The Token name set is persisted as subdomains of the **.tkn.eth** domain

For instance, the contract for USDC can be found at [usdc.tkn.eth](https://etherscan.io/address/usdc.tkn.eth) and Wrapped Bitcoin at [wbtc.tkn.eth](https://etherscan.io/address/wbtc.tkn.eth)

Descriptive metadata can be found on chain in each token's ENS resolver. Including **name, description, avatar, url, decimals, twitter, & github**.

The Ticker.sol contract exposes additional queries; such as: fetch all metadata, and token balance for address. Ticker.sol is hosted at [tkn.eth](https://etherscan.io/address/tkn.eth#code)

### Quickstart

#### Get token contract address

Retrieves the address where the token contract resides.

Useful for converting a user supplied token symbol into an actionable contract.

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
await provider.resolveName("uni.tkn.eth");
// '0x1f9840a85d5aF5bf1D1762F925BDADdC4201F984'

{% endtab %}

{% tab title="Web3.js" %}

web3.eth.ens.getAddress('uni.tkn.eth').then(function (address) {
  console.log(address);
  // '0x1f9840a85d5aF5bf1D1762F925BDADdC4201F984'
})

{% endtab %}

{% tab title="Solidity" %}

pragma solidity ^0.8.4;

contract HelloTicker {
    ITicker ticker;
    constructor() {
        ticker = ITicker(0x8F5007bCDC1870531029C194b31AE1e6005bc30d); 
    }
    
    // Get an account's balance from a ticker symbol
    function balanceForAddress(address user, string calldata tickerSymbol) public view returns (uint) {
        // Fetch the token contract address with ticker:
        address contractAddress = ticker.addressFor(tickerSymbol);
        IERC20 tokenContract = IERC20(contractAddress);
        return tokenContract.balanceOf(user);
    }
}
// Required Interfaces
interface ITicker {
    struct Metadata {
        address contractAddress;
        string name;
        string url;
        string avatar;
        string description;
        string notice;
        string twitter;
        string github;
    }
    function addressFor(string calldata _name) external view returns (address);
    function infoFor(string calldata _name) external view returns (Metadata memory);
    function gasEfficientFetch(bytes32 namehash) external view returns (address);
    function balanceWithTicker(address user, string calldata tickerSymbol) external view returns (uint);
}

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
}


{% endtab %}
{% endtabs %}
