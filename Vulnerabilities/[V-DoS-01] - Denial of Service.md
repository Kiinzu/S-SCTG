# Denial of Service

## Summary

Just like things in Web2, when the availability of a system is compromised that's what we called `Denial of Service`. In Smart Contract, it works just the same, if a function or operation in smart contract is compromised and cannot be accessed by others, that's a `Denial of Service (DoS)`. There are many ways to make a `DoS` in Smart contract, be it to render the contract function unable to be done, exhausting the contract out of gas, looping over unbounded arrays, and the list goes on.

## Test Objectives

- Ensure that functions in the smart contract doesn't introduce DoS Potential.
- Ensure access control is properly applied.

## How to Test

To check the smart contract for a possible Denial of Service issue, we can follow these simple checklist

1. **Input Validation**

It's a common practice to validate every input and treat every input as a "malicious" input, if for an example an input of arrays is expected but the arrays loops infinitely, it will make the transaction out of gas and making the function always failt o execute.

2. **Interaction Check**

Check who is allowed to interact with the Function within the smart contract, if only EOA a check of `msg.sender == tx.origin` can be used, if everyone is allowed to interact with the function, essential checks still need to be done before letting the function continue, preventing an unwanted entity to interact with the smart contract.

3. **Failure Handler**

A good smart contract doesn't only good at handling successful transaction, but also failure. Denial of Service most of the time related with the smart contract unable to continuously handle failure.

4. **Setting Limits on Gas**

Although less people use and pay attention to this, limiting the gas for a function with the `worst scenario` approach could save smart contract from being DoS-ed.

## Example

Let's take an example from [Solidity by Example - King of Ether](https://solidity-by-example.org/hacks/denial-of-service/). In this example, there is a smart contract which goals is to remain as the king as long as possible, if possible- for eternity.

```solidity
pragma solidity ^0.8.28

contract KingOfEther {
    address public king;
    uint256 public balance;

    function claimThrone() external payable {
        require(msg.value > balance, "Need to pay more to become the king");

        (bool sent,) = king.call{value: balance}("");
        require(sent, "Failed to send Ether");

        balance = msg.value;
        king = msg.sender;
    }
}
```

There is no limitation here who can interact with the smart contract, and if we see the logic here, if someone want to replace the old king, they can just pay a bigger amount to the `claimThrone()` function and the old king balance will be refunded., so the idea to become the eternal king is pretty obvious here.

We just need to create a smart contract that interact with the `clainThrone()` function and pay a larger sum of Ether to it and of course, we must not specify any `receive()` or `fallback()` function- or alternatively, if we specify it, everytime it receive ether, we are just going to make it revert.

```solidity
receive() external payable{
    revert();
}
```

Thus, everytime someone try to clain the throne by paying a bigger amount that our deposit, it won't go through, since everytime we receive the refund, it will always revert.

## References

- [Denial of Serice (DoS) Attack in Smart Contracts](https://www.nethermind.io/blog/denial-of-service-dos-attacks-in-smart-contracts#:~:text=Denial%20of%20Service%20(DoS)%20in%20the%20Web3%20realm&text=A%20DoS%20attack%20against%20a,intended%2C%20either%20temporarily%20or%20permanently.)
- [Common Vulnerabilities in Solidity: Denial of Service](https://www.slowmist.com/articles/solidity-security/Common-Vulnerabilities-in-Solidity-Denial-of-Service-DOS.html)
- [Vulnerability: Denial of Service](https://owasp.org/www-project-smart-contract-top-10/2023/en/src/SC06-denial-of-service-attacks.html)
- [Denial of Service (DoS) Attacks](https://www.quillaudits.com/blog/web3-security/denial-of-service-on-smart-contracts)
- [Denial of Service by Example](https://solidity-by-example.org/hacks/denial-of-service/)