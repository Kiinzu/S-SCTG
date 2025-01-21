# Cross-Function Reentrancy

## Summary

`Cross-Function Reentrancy` is a reentrancy that is done by calling a different function after the callback happened, effectively using the **not-yet-updated** state to gain advantages.

Let's see the example by [Smart Contract Security Field Guides](https://scsfg.io/hackers/reentrancy/#cross-function-reentrancy) about `Cross-Function Reentrancy`

```solidity
contract Vulnerable {
    mapping (address => uint) private balances;

    function transfer(address to, uint amount) public {
        if (balances[msg.sender] >= amount) {
        balances[to] += amount;
        balances[msg.sender] -= amount;
        }
    }

    function withdraw() public {
        uint amount = balances[msg.sender];
        (bool success, ) = msg.sender.call.value(amount)("");
        require(success);
        balances[msg.sender] = 0;
    }
}
```

In the `withdraw()` above clearly has a CEI pattern issues, which it effect the balance after the call is success. This time, instead of just abusing the `withdraw()` function, the Attacker can duplicate the money by using the `transfer()` function too. Here is how

1. Attacker call `withdraw()`
2. Using the **not-yet-updated** `balance`, he can then transfer some money to another person or maybe another smart contract.

The main points here, is that due to the `balance` not getting an immediate effect after the `withdraw()` was called, the **not-yet-updated** `balance` will be useable in the `transfer()` function.

## References

- [Reentrancy](https://scsfg.io/hackers/reentrancy/)
- [The Ultimate Guide to Reentrancy](https://medium.com/immunefi/the-ultimate-guide-to-reentrancy-19526f105ac)