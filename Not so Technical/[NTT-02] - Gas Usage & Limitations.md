# Gas Usage & Limitation

## Summary

Every operations in solidity cost gas, the more efficient an operation can be, the more money ones saves. High gas usage can make transactions prohibitively expensive, while exceeding the block gas limit can render the ocntract unusable.

## Test Objectives

- Ensure that operations run without any chance of failures due to gas limit.
- Ensure that operations is gas-optimized.
- Prevent loops and scalability issues due to inputs.

## How to Test

There are several ways to test about the gas usage on a smart contract functions:

1. **Validate Gas Limits**

We can validate that complex functions do not exceed the block gas limits of ~30M gas.

2. **Measure Gas Usage on testing**

In the test script, we may see how many gas is used by the functions in total or we can test it using tools like `hardhat-gas-reporter` or Foundry Forge's built-in `vm.recordGas()` and `vm.getGas()`.

3. **Optimization**

Optimization in Solidity Smart Contract can be done by using `Yul`. It's a low-level programming language design for EVM that gives developers finer control over the operations.

## Tools
- Foundry
- Hardhat
- [Slither](https://github.com/crytic/slither)

## References
- [Understanding Solidity Gas Optimization](https://101blockchains.com/top-solidity-gas-optimization-techniques/)
- [Expert Gas Optimization](https://www.alchemy.com/overviews/solidity-gas-optimization)
- [Book of Solidity Gas Optimization](https://www.rareskills.io/post/gas-optimization)