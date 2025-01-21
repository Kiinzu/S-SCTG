# Unchecked Returns Values

## Summary

Some of the function that is used in solidity might returns either Error, Revert or even values. If a return values from a core function of the contract is not checked, the operations might be vulnerable to `Unchecked Return Values` and resulting in many outcome like loss of funds, Denial of Service, and many more. 

## Test Objectives

- Ensure that return values are checked.

## How to Test

This is more likely to happends on a contract that handles Ether, to be more specific, transfering Ether. Solidity has 3 built-ins that allow to transfer ether, they are `transfer`, `send` and `call`, the current best practice is to use `call()`.

The difference between `transfer`, `send` and `call`.
- `transfer`: Consume 2300 gas, returns `error` when failed
- `send`: Consume 2300 gas, returns `boolean`
- `call`: Forward gas / Set gas, returns `boolean`

When transfering Ether, you will most likely to see one of these code in the codebase

```solidity
// Bad Example
// Unchecked external calls
function withdraw() public{
    uint toWithdraw = balanceOf[msg.sender];
    (bool sent, ) = msg.sender.call{value: toWithdraw}("");
}

// Good Example
// Return values are checked
function withdraw() public{
    uint toWithdraw = balanceOf[msg.sender];
    (bool sent, ) = msg.sender.call{value: toWithdraw}("");
    require(sent)
}
```

Why is the bottom one a good example? It's because it checks if the transfer was successful or not, if it wasn't, the transaction will revert, thus doesn't change the state variables, in this case the `balanceOf[msg.sender]`.

## References

- [Solidity Security Vulnerability: Unchecked Return Values](https://www.immunebytes.com/blog/solidity-security-vulnerability-unchecked-return-values/)
- [King of Ether Incident](https://www.kingoftheether.com/postmortem.html?source=post_page-----fe794a7cdb6f--------------------------------)
- [Unchecked call return value solidity security ](https://sm4rty.medium.com/unchecked-call-return-value-solidity-security-1-fe794a7cdb6f)