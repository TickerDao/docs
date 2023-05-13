# Querying the Dataset

## Documentation

The Token Name Service uses [ENS](https://ens.domains/), and can be queried anywhere .eth domains are supported

The Token name set is persisted as subdomains of the **.tkn.eth** domain

For instance, the contract for USDC can be found at [usdc.tkn.eth](https://etherscan.io/address/usdc.tkn.eth) and Wrapped Bitcoin at [wbtc.tkn.eth](https://etherscan.io/address/wbtc.tkn.eth)

Descriptive metadata can be found on chain in each token's ENS resolver. Including **name, description, avatar, url, decimals, twitter, & github**.

The Ticker.sol contract exposes additional queries; such as: fetch all metadata, fetch token contracts on all chains, and token balance for address. TNS.sol is hosted at [tkn.eth](https://etherscan.io/address/tkn.eth#code)

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
    function infoFor(string calldata _name) external view returns (TokenInfo memory);
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

#### **Get single datapoint**

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
const interface_avi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressesFor","outputs":[{"components":[{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.TokenAddresses","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"dataFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"},{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"_chainId","type":"uint256"},{"internalType":"string","name":"_name","type":"string"}],"name":"getContractForChain","outputs":[{"internalType":"bytes","name":"","type":"bytes"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"}],"internalType":"struct TNS.TokenInfo","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
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
    function infoFor(string calldata _name) external view returns (TokenInfo memory);
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

#### **Get all token info**

Retrieve all datapoints for a token, excluding the sidechain contract addresses.

Useful for fetching all token data in a single query.

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
let abi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressesFor","outputs":[{"components":[{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.TokenAddresses","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"dataFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"},{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"_chainId","type":"uint256"},{"internalType":"string","name":"_name","type":"string"}],"name":"getContractForChain","outputs":[{"internalType":"bytes","name":"","type":"bytes"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"}],"internalType":"struct TNS.TokenInfo","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
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
const interface_abi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressesFor","outputs":[{"components":[{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.TokenAddresses","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"dataFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"},{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"_chainId","type":"uint256"},{"internalType":"string","name":"_name","type":"string"}],"name":"getContractForChain","outputs":[{"internalType":"bytes","name":"","type":"bytes"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"}],"internalType":"struct TNS.TokenInfo","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
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
    function infoForTicker(string calldata tickerSymbol) public view returns (ITKN.TokenInfo memory info) {
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
    function infoFor(string calldata _name) external view returns (TokenInfo memory);
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

#### **Get all token info and sidechain contract addresses**

Retrieve all token info and it's sidechain contract addresses.

Useful for fetching all known data about a token in a single query.

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
let abi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressesFor","outputs":[{"components":[{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.TokenAddresses","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"dataFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"},{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"_chainId","type":"uint256"},{"internalType":"string","name":"_name","type":"string"}],"name":"getContractForChain","outputs":[{"internalType":"bytes","name":"","type":"bytes"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"}],"internalType":"struct TNS.TokenInfo","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
let tkn = new ethers.Contract("tkn.eth", abi, provider)
let data = await tkn.dataFor("uni")
// Returns an object with all token data and addresses: 
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
// data.arb1_address: "0xFa7F8980b0f1E64A2062791cc3b0871572f1F7f0"
// data.avaxc_address: "0x8eBAf22B6F053dFFeaf46f4Dd9eFA95D89ba8580"
// data.base_addres: 
// data.bsc_address: "0xBf5140A22578168FD562DCcF235E5D43A02ce9B1"
// data.cro_address: 
// data.ftm_address: 
// data.gno_address: "0x4537e328Bf7e4eFA29D05CAeA260D7fE26af9D74"
// data.matic_address: "0xb33EaAd8d922B1083446DC23f610c2567fB5180f"
// data.near_address: "1f9840a85d5af5bf1d1762f925bdaddc4201f984.factory.bridge.near"
// data.op_address: "0x6fd9d7AD17242c41f7131d257212c54A0e816691"
// data.sol_address: 
// data.trx_address: 
// data.zil_address: 
// data.goerli_address:  
// data.sepolia_address:  
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
    function dataForTicker(string calldata tickerSymbol) public view returns (ITKN.Metadata memory info) {
        ITKN.Metadata memory data = tkn.dataFor(tickerSymbol);
        return data;
    }
}
// Required Interfaces
interface ITKN {
    struct Metadata {
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
        address payable arb1_address;
        address payable avaxc_address;
        address payable base_address;
        address payable bsc_address;
        address payable cro_address;
        address payable ftm_address;
        address payable gno_address;
        address payable matic_address;
        bytes near_address;
        address payable op_address;
        bytes sol_address;
        bytes trx_address;
        bytes zil_address; 
        address payable goerli_address; 
        address payable sepolia_address; 
    }
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
    struct TokenAddresses {
        address payable arb1_address;
        address payable avaxc_address;
        address payable base_address;
        address payable bsc_address;
        address payable cro_address;
        address payable ftm_address;
        address payable gno_address;
        address payable matic_address;
        bytes near_address;
        address payable op_address;
        bytes sol_address;
        bytes trx_address;
        bytes zil_address; 
        address payable goerli_address; 
        address payable sepolia_address; 
    }
    function addressFor(string calldata _name) external view returns (address);
    function infoFor(string calldata _name) external view returns (TokenInfo memory);
    function addressesFor(string calldata _name) external view returns (TokenAddresses memory);
    function dataFor(string calldata _name) external view returns (Metadata memory);
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

#### **Get all sidechain contract addresses**

Retrieve all token sidechain contract addresses.

Useful for a token's contract addresses for all chains in a single query.

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
let abi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressesFor","outputs":[{"components":[{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.TokenAddresses","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"dataFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"},{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"_chainId","type":"uint256"},{"internalType":"string","name":"_name","type":"string"}],"name":"getContractForChain","outputs":[{"internalType":"bytes","name":"","type":"bytes"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"}],"internalType":"struct TNS.TokenInfo","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
let tkn = new ethers.Contract("tkn.eth", abi, provider)
let data = await tkn.addressesFor("uni")
// Returns an object with all token addresses: 
// data.arb1_address: "0xFa7F8980b0f1E64A2062791cc3b0871572f1F7f0"
// data.avaxc_address: "0x8eBAf22B6F053dFFeaf46f4Dd9eFA95D89ba8580"
// data.base_addres: 
// data.bsc_address: "0xBf5140A22578168FD562DCcF235E5D43A02ce9B1"
// data.cro_address: 
// data.ftm_address: 
// data.gno_address: "0x4537e328Bf7e4eFA29D05CAeA260D7fE26af9D74"
// data.matic_address: "0xb33EaAd8d922B1083446DC23f610c2567fB5180f"
// data.near_address: "1f9840a85d5af5bf1d1762f925bdaddc4201f984.factory.bridge.near"
// data.op_address: "0x6fd9d7AD17242c41f7131d257212c54A0e816691"
// data.sol_address: 
// data.trx_address: 
// data.zil_address: 
// data.goerli_address:  
// data.sepolia_address:  
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
    function addressesForTicker(string calldata tickerSymbol) public view returns (ITKN.TokenAddresses memory info) {
        ITKN.TokenAddresses memory data = tkn.addressesFor(tickerSymbol);
        return data;
    }
}
// Required Interfaces
interface ITKN {
    struct Metadata {
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
        address payable arb1_address;
        address payable avaxc_address;
        address payable base_address;
        address payable bsc_address;
        address payable cro_address;
        address payable ftm_address;
        address payable gno_address;
        address payable matic_address;
        bytes near_address;
        address payable op_address;
        bytes sol_address;
        bytes trx_address;
        bytes zil_address; 
        address payable goerli_address; 
        address payable sepolia_address; 
    }
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
    struct TokenAddresses {
        address payable arb1_address;
        address payable avaxc_address;
        address payable base_address;
        address payable bsc_address;
        address payable cro_address;
        address payable ftm_address;
        address payable gno_address;
        address payable matic_address;
        bytes near_address;
        address payable op_address;
        bytes sol_address;
        bytes trx_address;
        bytes zil_address; 
        address payable goerli_address; 
        address payable sepolia_address; 
    }
    function addressFor(string calldata _name) external view returns (address);
    function infoFor(string calldata _name) external view returns (TokenInfo memory);
    function addressesFor(string calldata _name) external view returns (TokenAddresses memory);
    function dataFor(string calldata _name) external view returns (Metadata memory);
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

#### **Get tokens owned by an address**

Lookup how many tokens an address owns, using a token symbol.

Helpful for looking up an ERC20 account balance without having to fetch the token contract address separately.

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
let abi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressesFor","outputs":[{"components":[{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.TokenAddresses","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"dataFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"},{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"_chainId","type":"uint256"},{"internalType":"string","name":"_name","type":"string"}],"name":"getContractForChain","outputs":[{"internalType":"bytes","name":"","type":"bytes"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"}],"internalType":"struct TNS.TokenInfo","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
let tkn = new ethers.Contract("tkn.eth", abi, provider)
let account = '0xAb5801a7D398351b8bE11C439e05C5B3259aeC9B'
let uniBalance = await tkn.balanceWithTicker(account, "uni")
// uniBalance: 1615576470000000000000
```
{% endtab %}

{% tab title="Web3.js" %}
```javascript
const interface = await web3.eth.ens.getAddress('tkn.eth')
const interface_abi = [{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressFor","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"addressesFor","outputs":[{"components":[{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.TokenAddresses","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"},{"internalType":"string","name":"tickerSymbol","type":"string"}],"name":"balanceWithTicker","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"dataFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"},{"internalType":"address payable","name":"arb1_address","type":"address"},{"internalType":"address payable","name":"avaxc_address","type":"address"},{"internalType":"address payable","name":"base_address","type":"address"},{"internalType":"address payable","name":"bsc_address","type":"address"},{"internalType":"address payable","name":"cro_address","type":"address"},{"internalType":"address payable","name":"ftm_address","type":"address"},{"internalType":"address payable","name":"gno_address","type":"address"},{"internalType":"address payable","name":"matic_address","type":"address"},{"internalType":"bytes","name":"near_address","type":"bytes"},{"internalType":"address payable","name":"op_address","type":"address"},{"internalType":"bytes","name":"sol_address","type":"bytes"},{"internalType":"bytes","name":"trx_address","type":"bytes"},{"internalType":"bytes","name":"zil_address","type":"bytes"},{"internalType":"address payable","name":"goerli_address","type":"address"},{"internalType":"address payable","name":"sepolia_address","type":"address"}],"internalType":"struct TNS.Metadata","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"bytes32","name":"namehash","type":"bytes32"}],"name":"gasEfficientFetch","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"_chainId","type":"uint256"},{"internalType":"string","name":"_name","type":"string"}],"name":"getContractForChain","outputs":[{"internalType":"bytes","name":"","type":"bytes"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"string","name":"_name","type":"string"}],"name":"infoFor","outputs":[{"components":[{"internalType":"address","name":"contractAddress","type":"address"},{"internalType":"string","name":"name","type":"string"},{"internalType":"string","name":"url","type":"string"},{"internalType":"string","name":"avatar","type":"string"},{"internalType":"string","name":"description","type":"string"},{"internalType":"string","name":"notice","type":"string"},{"internalType":"string","name":"version","type":"string"},{"internalType":"string","name":"decimals","type":"string"},{"internalType":"string","name":"twitter","type":"string"},{"internalType":"string","name":"github","type":"string"},{"internalType":"bytes","name":"dweb","type":"bytes"}],"internalType":"struct TNS.TokenInfo","name":"","type":"tuple"}],"stateMutability":"view","type":"function"}];
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
    function infoFor(string calldata _name) external view returns (TokenInfo memory);
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
