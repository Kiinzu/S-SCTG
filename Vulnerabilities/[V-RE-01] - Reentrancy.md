# Reentrancy

## Summary

Reentrancy is an attack name that consist of 2 words, `re` meaning doing something again and `entrance` which mean entry, well the name actually explains the vulnerability which one can control the flow of the program after getting a callback interaction after receiving Ether.

There are 4 types of reentrancy, 
1. [Single-Function Reentrancy]([V-RE-02]%20-%20Single-Function%20Reentrancy.md#single-function-reentrancy)
2. [Cross-Function Reentrancy]([V-RE-03]%20-%20Cross-Function%20Reentrancy.md#cross-function-reentrancy)
3. [Cross-Contract Reentrancy]([V-RE-04]%20-%20Cross-Contract%20Reentrancy.md#cross-contract-reentrancy)
4. [Read-Only Reentrancy]([V-RE-05]%20-%20Read-Only%20Reentrancy.md#read-only-reentrancy)

In this section of the S-SCTG, we are going to mainly discuss the main cause of Reentrancy and how to finds them. The 4 types of reentrancy will be discussed in their respectful section.

## Test Objectives

- Ensure that `Check-Effect-Interaction (CEI)` pattern is used.
- Ensure that there is a mechanism to prevent Reentrancy
- Ensure that the use of `safeMint` has some additional protection 

## How to Test

Reentrancy is often happened when the devs doesn't follow the CEI Pattern or there is a gap where the attacker could use to interact after the callback, here is some of the thing that need to be checked on the smart contract to look for Reentrancy Attacks

1. **CEI Pattern**

Smart contract will usualy have this kind of pattern in the function which handles Ether, like transfer, withdraw, deposit, etc. 

The pattern starts with `Check`- meaning it will check for requirements before contine to changing the State variables or values. Next is the `Effect`, in this stage, if for example the function is withdraw, after checking some prequisite like if the withdrawer has enough balance, it will then deduct the balance in this stage. The last one is `Interaction`, in this phase continuing from the deduction of balance, the contract will the "interact" with the withdrawer by sending the balance to them. Here is an example of a correct CEI Pattern in a function.

```solidity
function withdraw(uint256 _amount) public{
    # Check
    require(balance[msg.sender] <= _amount); 

    # Effect
    balance[msg.sender] -= _amount;

    # Interaction
    (bool sent, ) = payable(msg.sender).call{value: _amount}("");
    require(sent);
}
```

2. **Mechanism To Prevent Reentrancy**

There are some library that is created to prevent the Reentrancy attack, just like [Openzeppelin ReentrancyGuard](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/ReentrancyGuard.sol) or any manual mechanism that is created to prevent another re-entrant to the contract.

3. **External call from Standards**
Some Standards like ERC-721 and ERC-1155, has the function of `safe*` (safe something), this function if not protected carefully, could introduce reentrancy attacks. In other standards like ERC-777 and ERC-223, the `transfer` might also introduce a reentrancy attack if not protected correctly.

## References
- [Openzeppelin ReentrancyGuard](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/ReentrancyGuard.sol)
- [The Ultimate Guide to Reentrancy](https://medium.com/immunefi/the-ultimate-guide-to-reentrancy-19526f105ac)
- [Reentrancy](https://scsfg.io/hackers/reentrancy/)
