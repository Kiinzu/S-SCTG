# Unprotected Functions

## Summary

Functions that act as the role feature of a smart contract need to be proctected, may it be using a role base accessed or requirements to access the functions.

## Test Objectives

- Ensure that core/administrative functions is proctected
- Ensure that proper checks is done for core/administrative functions

## How to Test

First thing to do is to identify the core functions that the smart contract has and make sure a proper checks like `reuire` or `modifier` are implemented for the functions. The Core function of a contract may be differ, for example a smart contract that has a bank-like feature means that the core functions must be the deposit-withdraw feature, on the other hand an NFT Minter contract- the core function is the one doing the mint and keeping track on who owns the NFT.

## Example

Let's see an example of a smart contract that is managed by someone called `admin`.

```solidity

pragma solidity ^0.8.0;

contract AdminManager{
    address public admin;

    constructor() {
        admin = msg.sender;
    }

    ...Rest of Code

    function changeAdmin(address _newAdmin) public{
        admin = _newAdmin;
    }
}
```

In the example above, the administrative privilege can be change using the `changeAdmin()` function, but the function doesn't seem to have any protection to it. This makes anyone able to call the function without any proper authentication.

## References

- [Access Control Vulnerabilities in Smart Contract](https://metaschool.so/articles/access-control-vulnerabilities-in-smart-contracts#Real-World_Examples_of_Access_Control_Vulnerabilities)
- [Access Control Vulnerabilities in Solidity Smart Contracts](https://www.immunebytes.com/blog/access-control-vulnerabilities-in-solidity-smart-contracts/)