# Weak Identifier

## Summary

Some smart contract needs a unique way to identify a person or who is interacting with it, most of the time it will be recorded using the `mapping` function, the classic key and value mechanism. This method is preferred, however if the identifier like the key is predictable or can be easily manipulate, it renders the mechanism to be insecure.

## Test Objectives

- Ensure that identifier is unique
- Ensure that identifier is not predictable

## How to Test

Some best practice and the easiest way to create a unique identifier is to use the address `msg.sender` as the key, but not limited to other customization using built-in function like `abi.encode` or `keccak256`, but there are always things to check:

1. **Identifier using Counter**
This type of identifier is okay for keeping counts to things that such like how many NFT has been minted and the id, how many transfer has a user made, etc. It is not recommended to use a Counter method as a form of authentication since it's very predictable.

2. **`abi.encodePacked()`**
Although it's very common to use the `abi.encodePacked` with `keccak256`, it's not really recommended because it's more suscetible to **hash collision** vulnerability

3. **block global variable**
Solidity comes with many global variable, one of them is the `block` global variable, it has the `number`, `timestamp`, `prevrandao`, and many more. Using them as an addition to the identifier might arise an issue related with `insecure randomness` since they are easy to predict.

4. **`tx.origin`**
Solely relying on `tx.origin` as an identifier is insecure and considered an anti-pattern, the reason behind this is because `tx.origin` points to the EOA who initiate the transaction which is relatively easy to be done using a phishing attacks where `tx.origin` is a trusted address but originates froma  malicious contract.

## Examples

Taking an example from [Casino Heist - Singular Identity](https://github.com/Kiinzu/Casino-Heist/blob/main/Challenges/Common/C-Singular%20Identity/contracts/Singularity.sol), we are going to see why using `abi.encodePacked()` is a bad idea for an identifier. We are going to see the 2 functions that are responsible as the sink and the generator function then what the damages they brings here

```solidity
function getIdentity(string memory _firstName, string memory _lastName) public pure returns(bytes memory){
        return abi.encodePacked(_firstName, _lastName);
    }
```

The `getIdentity()` function has 2 input of string, `_firstName` and `_lastName` then it joins the string using `abi.encodePacked()`

```solidity
mapping(bytes=>uint256) public balanceOf;
mapping(string=>mapping(string=>address)) public Member;

function register(string memory _firstName, string memory _lastName) public payable{
        require(Member[_firstName][_lastName] == address(0), "Already Registered");
        require(msg.value > 0);
        bytes memory code = getIdentity(_firstName, _lastName);
        balanceOf[code] += msg.value;
        Member[_firstName][_lastName] = msg.sender;
    }
```

The `register()` function has 2 inputs of string, which then will be registered as `Member` and then it uses the bytes from the `getIdentity()` return value as the identifier to map the balance.

The security issue here lies in the usage of the `abi.encodePacked()` which we are going to simulate here, let's say the name of a person is `Alice White`

```
Step 1: Join the String - AliceWhite
Step 2: encode packed it - 0x416c6963655768697465
```

The issue with `abi.encodePacked()` is that it just going to join all the args, which mean we can easily make a hash collisions for `Alice White` with some combination like `AliceWh ite`, `Ali ceWhite`, and many more since the join result will still be `AliceWhite`. The real damages here is the function `withdraw()`

```solidity
function withdraw(string memory _firstName, string memory _lastName, uint256 _amount) public{
        require(Member[_firstName][_lastName] == msg.sender, "You cannot withraw other people money!");
        bytes memory code = getIdentity(_firstName, _lastName);
        require(balanceOf[code] - _amount >= 0, "You don't have this kind of money!");
        balanceOf[code] -= _amount;
    } 
```

If we can find a valid name and do a hash collision for the name, we can actually draw their balances since the mapping for `balanceOf` will search and return the balance of the corresponding `hash`.

## References
- [Casino Heist - Singular Identity](https://github.com/Kiinzu/Casino-Heist/blob/main/Challenges/Common/C-Singular%20Identity/contracts/Singularity.sol)