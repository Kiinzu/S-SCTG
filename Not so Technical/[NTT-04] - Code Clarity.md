# Code Clarity

## Summary

Code Clarity is a must in a project, this can be also referred as the documnetation of the project itself. The more detail the code is written, the better for the next devs to continue the project and cut some times for the Auditor to actually start auditing and finding bugs in the code. This doesn't only mean for the code to be write neatly, but also the explanation of the code like the documentation and comments, to really give a precise information of the components.

## Test Objectives

- Ensure that documentation is clear and explainatory.
- Ensure that comments are clear and explanatory.
- Ensure that comments and the Logic of the code are in sync.

## How to Test

Testing the code clarity may differ to each people, but to make it up to the standard, here are some checks that can be done to determined it:

1. **Documentation**

The documnetation must include the idea of the codebase, the usage of the main contracts, a compact explanation about the main functions, roles, project installation, audit scopes and intended interactions.  

Example [FF#17 Dussehra](https://github.com/Cyfrin/2024-06-Dussehra/blob/main/README.md)

2. **Comments**

Comments are meant to make anyone who has zero knowledge of the contract to get a better and quicker understanding about the contract, so each comments need to be precise and explanatory.

Example [OddNiBank.sol](https://github.com/Kiinzu/ODD-NI-bank/blob/master/src/OddNiBank.sol)

```solidity
contract OddNiBank{
    // The Contract will have some constants
    uint256 public constant BONUS_TO_PAY = 1 ether;

    // Security Meassure
    // reentrancyLock - This method will lock every function in this contract upon withdrawal, ensures that 
    //                  No Reentrancy is possible to drain the contract, or access to any crucial function
    //                  within this contract.
    bool public reentrancyLock;
    
    // The Contract will have some mappings:
    // assetInvested - Asset that are currently deposited to the bank
    // bonusTaken - Map if member already taken bonus
    // members - If an address is a member of the bank  
    mapping (address => uint256) public assetInvested;
    mapping (address => bool) public bonusTaken;
    mapping (address => bool) public isMember;
    .
    .
    .
}
```

**NOTE**

In the smart contract audit, usually there is a channel where auditors can communicate with the developer and ask questions, some general question can be asked in that forum while some more private bug-likely question should be asked in more private channel.

## References
- [FF#17 Dussehra](https://github.com/Cyfrin/2024-06-Dussehra/blob/main/README.md)
- [OddNiBank - OddNiBank.sol](https://github.com/Kiinzu/ODD-NI-bank/blob/master/src/OddNiBank.sol)