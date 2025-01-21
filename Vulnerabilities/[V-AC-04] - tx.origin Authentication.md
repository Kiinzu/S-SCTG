# `tx.origin` Authentication

## Summary

The `tx.origin` solidity global variable is a variable that points out to the origin of a transaction or the EOA (person) who initiate the transaction. Using the `tx.origin` to do the authentication is considered to be flawed as we've discussed in [Weak Identifier]([V-AC-01]%20-%20Weak%20Identifier.md#weak-identifier) due to phishing attack.

## Test Objectives

- Ensure that `tx.origin` is not used as a form of Authentication.

## How to Test

To check for this security issue, we can simply check the authentication used in the smart contract like their `require`(s) and `modifier`, if any of them are using the `tx.origin` as the authentication, such as these:

```solidity
require(tx.origin == owner, "Not Owner");

modifier onlyOwner(){
    require(tx.origin == owner, "Not Owner");
    _;
}
```

## Example

It's hard to replicate a Phishing attack but here is an example of it, let's say we have this smart contract.

```solidity
pragma solidity ^0.8.0;

contract AdminManager{
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    ...Rest of Logic

    function withdrawAllTo(address _to){
        require(tx.origin == owner, "Only Owner can Withdraw");
        require(_to != address(0));
        (bool sent, ) = payable(_to).call{value: address(this).balance}("");
        require(sent, "Withdraw faileds.");
    }
}
```

The contrat above need the authorization from `owner` to be able to call the `withdrawAllTo()`, let's say we create an attack contract that has the function below

```solidity
function trickAdmin() public{
    AM.withdrawAllTo(address(this));
}
```

If we manage to trick the owner to somehow connect sign this function call with his wallet, we are going to have the `msg.sender` to points to our attack contract but the `tx.origin` is points to the actual owner since he is the one who initate the transaction, thus bypassing the checks.

## References

- [Smart Contract Security: tx.oriigin Authorization Attack Vectors](https://medium.com/coinmonks/smart-contract-security-tx-origin-authorization-attack-vectors-027730ae601d)
- [Understanding Phishing with tx.origin in Solidity](https://www.infuy.com/blog/understanding-phishing-with-tx-origin-in-solidity/)
- [Issues with Authorization Using tx.origin](https://neptunemutual.com/blog/issues-with-authorization-using-txorigin/)