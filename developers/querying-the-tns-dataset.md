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
```
{% endtab %}

{% tab title="Web3.js" %}
```javascript

web3.eth.ens.getAddress('uni.tkn.eth').then(function (address) {
  console.log(address);
  // '0x1f9840a85d5aF5bf1D1762F925BDADdC4201F984'
})
```
{% endtab %}

{% tab title="Solidity" %}
```solidity
pragma solidity ^0.8.4;

contract HelloTKN {
    ITKN tkn;
    constructor() {
        tkn = ITKN(0xc11BA824ab2cEA5E701831AB87ed43eb013902Dd); 
    }
    
    // Get an account's balance from a ticker symbol
    function balanceForAddress(address user, string calldata tickerSymbol) public view returns (uint) {
	// Fetch the token contract address with ticker:
	address contractAddress = tkn.addressFor(tickerSymbol);
        IERC20 tokenContract = IERC20(contractAddress);
        return tokenContract.balanceOf(user);
    }
}
// Required Interfaces
interface ITKN {
    struct TokenInfo {
        address contractAddress;
        string name;
        string url;
        string avatar;
        string description;
        string notice;
        string version;
        string decimals;
        string twitter;
        string github;
        bytes dweb;
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
```
{% endtab %}
{% endtabs %}

**Get single datapoint**

Get one of the datapoints available for each token.

Pulls a single datapoint, such as a name or description.

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const resolver = await provider.getResolver("uni.tkn.eth");
const description = await resolver.getText("description");
// "Protocol used to exchange cryptocurrencies and tokens"
```
{% endtab %}

{% tab title="Web3.js" %}
```javascript
const interface = await web3.eth.ens.getAddress('tkn.eth')
const interface_abi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"}],"internalType":"struct Ticker.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
const contract = await (new web3.eth.Contract(interface_abi, interface));

const info = await contract.methods.infoFor("uni").call()
console.log(info.description)
// "Protocol used to exchange cryptocurrencies and tokens"
```
{% endtab %}

{% tab title="Solidity" %}
```solidity
pragma solidity ^0.8.4;

contract HelloTKN {
    ITKN tkn;
    constructor() {
        tkn = ITKN(0xc11BA824ab2cEA5E701831AB87ed43eb013902Dd); 
    }

    // Get a token's description
    function descriptionForTicker(string calldata tickerSymbol) public view returns (string memory description) {
        return tkn.infoFor(tickerSymbol).description;
    }
}
// Required Interfaces
interface ITKN {
    struct TokenInfo {
        address contractAddress;
        string name;
        string url;
        string avatar;
        string description;
        string notice;
        string version;
        string decimals;
        string twitter;
        string github;
        bytes dweb;
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
```
{% endtab %}
{% endtabs %}

**Get all token info**

Retrieve all datapoints for a token, excluding the sidechain contract addresses.

Useful for fetching all token data in a single query.

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
let abi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"}],"internalType":"struct Ticker.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
let tkn = new ethers.Contract("tkn.eth", abi, provider)
let data = await tkn.infoFor("uni")
// Returns an object with all token data: 
// data.name: "Uniswap"
// data.avatar: "https://bafybeiclyzf4rogl67kd42ss2z2mnf4o3pf5s2aginvsxk7iomlelxhsdi.ipfs.dweb.link/"
// data.contractAddress: "0x1f9840a85d5aF5bf1D1762F925BDADdC4201F984"
// data.description: "Protocol used to exchange cryptocurrencies and tokens"
// data.url: "https://app.uniswap.org/"
// data.github: "Uniswap"
// data.twitter: "uniswap"
// data.notice: ""
// data.version: "0.0.3"
// data.decimals: "18"
// data.dweb: 0xe301017012201096cbbb08ab6e904227565788ca404d17a28b80e8af2348552b65d7151c101c
// (Convert dweb-bytes to IPFS CID with npm 'content-hash': https://github.com/pldespaigne/content-hash#contenthashdecode-contenthash----string)
```
{% endtab %}

{% tab title="Web3.js" %}
```javascript
const interface = await web3.eth.ens.getAddress('tkn.eth')
const interface_abi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"}],"internalType":"struct Ticker.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
const contract = await (new web3.eth.Contract(interface_abi, interface));
const info = await contract.methods.infoFor("uni").call()
// Returns an object with all token data: 
// data.name: "Uniswap"
// data.avatar: "https://bafybeiclyzf4rogl67kd42ss2z2mnf4o3pf5s2aginvsxk7iomlelxhsdi.ipfs.dweb.link/"
// data.contractAddress: "0x1f9840a85d5aF5bf1D1762F925BDADdC4201F984"
// data.description: "Protocol used to exchange cryptocurrencies and tokens"
// data.url: "https://app.uniswap.org/"
// data.github: "Uniswap"
// data.twitter: "uniswap"
// data.notice: ""
// data.version: "0.0.3"
// data.decimals: "18"
// data.dweb: 0xe301017012201096cbbb08ab6e904227565788ca404d17a28b80e8af2348552b65d7151c101c
// (Convert dweb-bytes to IPFS CID with npm 'content-hash': https://github.com/pldespaigne/content-hash#contenthashdecode-contenthash----string)
```
{% endtab %}

{% tab title="Solidity" %}
```solidity
pragma solidity ^0.8.4;

contract HelloTKN {
    ITKN tkn;
    constructor() {
        tkn = ITKN(0xc11BA824ab2cEA5E701831AB87ed43eb013902Dd); 
    }
	
    // Get the full dataset for a ticker 
    function dataForTicker(string calldata tickerSymbol) public view returns (ITicker.Metadata memory info) {
        ITKN.TokenInfo memory data = tkn.infoFor(tickerSymbol);
        return data;
    }
}
// Required Interfaces
interface ITKN {
    struct TokenInfo {
        address contractAddress;
        string name;
        string url;
        string avatar;
        string description;
        string notice;
        string version;
        string decimals;
        string twitter;
        string github;
        bytes dweb;
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
```
{% endtab %}
{% endtabs %}

**Get all token info**

Retrieve all meta datapoints for a token.

Useful for fetching all token data in a single query.

{% tabs %}
{% tab title="Solidity" %}
```solidity
pragma solidity ^0.8.4;

contract HelloTKN {
    ITKN tkn;
    constructor() {
        tkn = ITKN(0xc11BA824ab2cEA5E701831AB87ed43eb013902Dd); 
    }
	
    // Get the full dataset for a ticker 
    function dataForTicker(string calldata tickerSymbol) public view returns (ITicker.Metadata memory info) {
        ITKN.TokenInfo memory data = tkn.infoFor(tickerSymbol);
        return data;
    }
}
// Required Interfaces
interface ITKN {
    struct TokenInfo {
        address contractAddress;
        string name;
        string url;
        string avatar;
        string description;
        string notice;
        string version;
        string decimals;
        string twitter;
        string github;
        bytes dweb;
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
```
{% endtab %}
{% endtabs %}

**Get tokens owned by an address**

Lookup how many tokens an address owns, using a token symbol.

Helpful for looking up an ERC20 account balance without having to fetch the token contract address separately.

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
let abi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"}],"internalType":"struct Ticker.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
let tkn = new ethers.Contract("tkn.eth", abi, provider)
let account = '0xAb5801a7D398351b8bE11C439e05C5B3259aeC9B'
let uniBalance = await tkn.balanceWithTicker(account, "uni")
// uniBalance: 1615576470000000000000
```
{% endtab %}

{% tab title="Web3.js" %}
```javascript
const interface = await web3.eth.ens.getAddress('tkn.eth')
const interface_abi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"}],"internalType":"struct Ticker.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
const contract = await (new web3.eth.Contract(interface_abi, interface));
const account = '0xAb5801a7D398351b8bE11C439e05C5B3259aeC9B'
const uniBalance = await contract.methods.balanceWithTicker(account, "uni").call()
// uniBalance: 1615576470000000000000
```
{% endtab %}

{% tab title="Solidity" %}
```solidity
pragma solidity ^0.8.4;

contract HelloTKN {
    ITKN tkn;
    constructor() {
        tkn = ITKN(0xc11BA824ab2cEA5E701831AB87ed43eb013902Dd); 
    }
	
    // Get the caller's balance of a token
    function myBalanceForTicker(string calldata tickerSymbol) public view returns (uint balance) {
        return tkn.balanceWithTicker(msg.sender, tickerSymbol);
    }
}
// Required Interfaces
interface ITKN {
    struct TokenInfo {
        address contractAddress;
        string name;
        string url;
        string avatar;
        string description;
        string notice;
        string version;
        string decimals;
        string twitter;
        string github;
        bytes dweb;
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
```
{% endtab %}
{% endtabs %}
