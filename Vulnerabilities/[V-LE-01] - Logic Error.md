# Logic Error

## Summary

Smart Contract is also a programable langauge, meaning it doesn't write itself, someone wrote it, and that's us, human. Sometimes, we might just forget something or making a mistake on the flow of the logic, making a vulnerability known as human error, or what we are going to discuss here logic errors or business logic error.

## Test Objectives

- Ensure that smart contract behaves like the intended behaviour.

## How to Test

To test this, there is no other way than `Writting a Good Test`. Yes, this is related to with the [Not-So-Technical - Test Coverage](../Not%20so%20Technical/[NTT-05]%20-%20Test%20Coverage.md#test-coverage), why? The more test the devs write, the more coverage it will have, and that's ensured the functions that the devs had written ,run properly.

On the other hand, on the POV of auditor, a good test coverage also eliminate times required for them to analyze and further understand the code and waht is the intended behaviour are. All of this, won't eliminate the possibility that the logic is flawed and the test that's written is also flawed, sometimes things just happened.

## Example

We are going to take a look at OWASP Smart Contract Top 10, the [Vulnerability - Logic Errors](https://owasp.org/www-project-smart-contract-top-10/2023/en/src/SC07-logic-errors.html).

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract LendingPlatform {
    mapping(address => uint256) public userBalances;
    uint256 public totalLendingPool;

    function deposit() public payable {
        userBalances[msg.sender] += msg.value;
        totalLendingPool += msg.value;
    }

    function withdraw(uint256 amount) public {
        require(userBalances[msg.sender] >= amount, "Insufficient balance");
        
        // Faulty calculation: Incorrectly reducing the user's balance without updating the total lending pool
        userBalances[msg.sender] -= amount;
        
        // This should update the total lending pool, but it's omitted here.
        
        payable(msg.sender).transfer(amount);
    }
}
```

given a smart contract like above, the `deposit()` function seems okay since it add the deposited balance to the `userBalances` and the `totalLendingPool`, but the `withdraw()` here is a bit off. After deducting the balance from the `userBalances[msg.sender]` mappings, it doesn't deduct the amount from the `totalLendingPool`, making a different in the total ether that the contract has with the `totalLendingPool` variable value.

## References
- [Vulnerability - Logic Errors](https://owasp.org/www-project-smart-contract-top-10/2023/en/src/SC07-logic-errors.html)
- [Smart Contract Security Risks](https://www.cobalt.io/blog/smart-contract-security-risks#:~:text=7.-,Logic%20Errors%20(Business%20Logic%20Vulnerabilities),funds%20or%20misallocation%20of%20tokens.)