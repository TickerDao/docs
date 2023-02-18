---
description: How to get TNS data quickly on the client or in Solidity
---

# Quickstart

### **Get token contract address**

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
Use `tns.addressFor(tickerSymbol)` to return the token contract address.&#x20;

See a deployable example contract below:

<pre class="language-solidity"><code class="lang-solidity">pragma solidity ^0.8.4;

contract HelloTicker {
    TNS tns;
    constructor() {
        tns = TNS(0x8F5007bCDC1870531029C194b31AE1e6005bc30d); 
    }
    
    // Get an account's balance from a ticker symbol
    function balanceForAddress(address user, string calldata tickerSymbol) public view returns (uint) {
	// Fetch the token contract address with ticker:
	
<strong>	address contractAddress = tns.addressFor(tickerSymbol);
</strong>	
        IERC20 tokenContract = IERC20(contractAddress);
        return tokenContract.balanceOf(user);
    }
}
// Required Interfaces
interface TNS {
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
</code></pre>
{% endtab %}
{% endtabs %}
