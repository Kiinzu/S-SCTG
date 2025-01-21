# Standards Misimplementation

## Summary

Ethereum has many standards, this include the `Ethereum Request for Comment (ERC)` and `Ethereum Improvement Proposals (EIP)`. They are an inovation that created to make the Ethereum Ecosystem better and enhance the real-world adoption of Ethereum, but misimplemented Standards could introduce various security issue.

## Test Objectives

- Ensure that deviation from Standards doesn't introduce security issue.
- Ensure that Standards is implemented correctly.

## How to Test

There are some easy ways to test for Standard implementation in a Solidity smart contract:

1. **Comparing with the Original Version**
We can simply compare the inherited function with the original code to find any deviation from the original version, this is usually marked with `override` in the function name.

2. **Changeable Values**
In some standards, there are some variables that need to be change according to the needs of each smart contract, we need to make sure that only the needed variable is changed.

3. **`override`**
As mentioned in the first (1) point, any changes from the original version need to be checked whether it introduce a security issues or not, normally it doesn't but some times it may introduce some new issues.

## Examples
The example is kinda hard to find so we are going to use an example from [Casino Heist - Unlimited Credit Line](https://github.com/Kiinzu/Casino-Heist/blob/main/Challenges/Common/C-Unlimited%20Credit%20Line/contracts/NewBank.sol)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import {IBetterERC20} from "./BetterERC20.sol";

contract NewBank is IBetterERC20{
    address public override owner;
    mapping(address => uint256) public override balanceOf;
    mapping(address => mapping(address => uint256)) public override allowance;

    string public override name = "NewBank Token";
    string public override symbol = "NBT";
    uint8 public override decimals = 18;

    constructor(uint256 _initialSupply) {
        owner = msg.sender;
        balanceOf[msg.sender] = _initialSupply;
    }

    function transfer(address _to, uint256 _value) external override returns (bool) {
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) external override returns (bool) {
        require(balanceOf[_from] >= _value, "Insufficient balance");
        require(allowance[_from][msg.sender] >= _value, "Allowance exceeded");
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        return true;
    }

    function approve(address _spender, uint256 _value) external override returns (bool) {
        allowance[msg.sender][_spender] = _value;
        return true;
    }

    function mint(uint256 _value) external override {
        require(msg.sender == owner, "Only Owner are allowed to mint!");
        balanceOf[msg.sender] += _value;
    }

    function burn(address _who, uint256 _value) external override {
        require(balanceOf[_who] <= _value, "Insufficient balance to burn");
        balanceOf[_who] += _value;
    }
    
}
```

In the ERC-20 contract above, we can see that there are 5 functions that is marked `override`, so we need to compare them to the original ERC-20 to find any anomalies. Fortunately, it's quite obvious ehre, where `mint()` is the one who add the balance to the address, but `burn()` in this contract actually allow us to give someone some balance, this is because instead of removing the balance, it actually adding it to the destined address. 

## References
- [Casino Heist - Unlimited Credit Line](https://github.com/Kiinzu/Casino-Heist/blob/main/Challenges/Common/C-Unlimited%20Credit%20Line/contracts/NewBank.sol)