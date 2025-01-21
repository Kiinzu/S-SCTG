# Integer Overflow & Underflow

## Summary

Just like another programming langauge, Solidity also has integer as a data type in it. With the progressing upgrade of solidity from 0.4.0 up to now standing at 0.8.29, vulnerability related to arithmetic like integer overflow and underflow is already fixed by the upgrade made at version 0.8.0, but that doesn't mean that the threat is fully eliminated.

## Test Objectives

- Ensure that `unchecked()` doesn't bring security issues
- Ensure that smart contract is written using solidity version 0.8.0 or higher.

## How to Test

To test for Integer Overflow and Underflow, there are some check that could be done to the codebase.

1. **Solidity Version**
Using a solidity version below 0.8.0 is dangerous as it could bring the Integer Underflow Overflow vulnerability on the data type if they are not using an extra library which is `safeMath`. Solidity 0.8.0 and above mitigate this by checking for Underflow and Overflow on each executions by default without the need of external library.

2. **`unchecked()`**
The `unchecked()` is a global function that tells the EVM not to check for the result of the arithmetic operations even if it creates an underflow or overflow. Thus, this global function is better to be used when the operations is likely to never ends with an underflow or overflow.

3. **Low-Level Operations**
Another way to bypass the checks in solidity 0.8.0 and above is to use a low level operation using yul as the EVM not checked whether an integer overflow or underflow occured during the operation in low-level.

4. **Shift Operators**
Using the shift operators, just like the low-level, it doesn't normally checked for integer underflow or overflow, making using a shift operators to calculate something might be prone to the vulnerability.

5. **Typecasting**
Just like another programming language, you can typecase a `uint` to `int` in Solidity. In some cases, when the typecasting from a bigger data type to a smaller one, an integer overflow or underflow may occure.

## Example

We are taking an example from [Casino Heist - Cheap Glitch](https://github.com/Kiinzu/Casino-Heist/blob/main/Challenges/Common/C-Cheap%20Glitch/contracts/Capitol.sol).

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Capitol{
    
    ...Rest of Code
    address public owner;
    mapping(address => uint256) public balanceOf;

    function depositCredit(uint256 _amount) public payable{
        require(_amount > 1 ether, "Minimum deposit is 1 ether");
        require(msg.value == _amount, "There seems to be a mismatch!");
        unchecked{
            balanceOf[msg.sender] += _amount;
        }
    }

    function withdrawCredit(uint256 _amount) public{
        require(_amount > 0, "Must be greater than zero!");
        unchecked{
            balanceOf[msg.sender] -= _amount;
        }
    }

    ...Rest of Code
}
```

The core function of the `Capitol Smart Contract` is the deposit-withdraw pair, the `depositCredit()` checks for the `_amount` and `msg.value` before adding it to the `balanceOf` mapping, but the `withdrawCredit` only checks for the `_amount` input before verifying the balance of the caller. 

The Issue here is the `unchceked()`, for the `depositCredit()` it might be kinda hard to reach the maximum value of `uint256` when adding the balance, but for the `withdrawCredit()`, since there is no validation for the `_amount` to be withdraw and the current balance that the caller has, the `unchecked()` actually introduce a integer underflow issues.

Everyone start with 0 balance on the `balanceOf` mapping, if they immediately use the `withdrawCredit()` function, like for example 1, the balance of their account will be the max value of `uint256` since `0 - 1` in `uint256` will result in `2^256 - 1` (max value of `uint256`) after the integer underflow.

## References

- [Casino Heist - Cheap Glitch](https://github.com/Kiinzu/Casino-Heist/blob/main/Challenges/Common/C-Cheap%20Glitch/contracts/Capitol.sol)
- [Integer Overflow and Underflow in Solidity](https://metaschool.so/articles/integer-overflow-and-underflow-in-solidity)
- [Vulnerability: Integer Overflow and Underflow](https://owasp.org/www-project-smart-contract-top-10/2023/en/src/SC02-integer-overflow-underflow.html)