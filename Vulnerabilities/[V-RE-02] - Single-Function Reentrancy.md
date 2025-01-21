# Single-Function Reentrancy

## Summary

`Single-Function Reentrancy` is a reentrancy that happened within a singular function which will be repeatedly called over and over again to gain some advantages for the attacker. Here is a small example

```solidity
contract SFReentrancy{
    mapping (address => uint256) public balanceOf;

    function deposit() public payable{
        ...Rest of Code
    }

    function withdrawAll() public{
        uint256 toWithdraw = balanceOf[msg.sender];
        (bool sent, ) = msg.sender.call{value: toWithdraw}("");
        require(sent, "Withdraw Failed");
        balanceOf[msg.sender] = 0;
    }
}
```

In the example above, the pattern of the `withdrawAll()` function is not the correct CEI Pattern. Let's say the contract allow smart contrat to interact with it too. If a malicious smart contract interact with it and have their `receive()` function to include some logic like

```solidity
receive() external payable{
    if(address(sfr).balance > 0){
        sfr.withdrawAll();
    }
}
```

When the attacker initiate the withdraw using the malicious contract and the contract receive the Ether and the condition of `receive()` is fulfilled, then the malicious contract will trigger another `withdrawAll()` until the SFReentrancy Contract balances is drained.

## References

- [Reentrancy](https://scsfg.io/hackers/reentrancy/)
- [The Ultimate Guide to Reentrancy](https://medium.com/immunefi/the-ultimate-guide-to-reentrancy-19526f105ac)