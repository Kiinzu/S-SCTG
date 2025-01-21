# Denial of Service

## Summary

Just like things in Web2, when the availability of a system is compromised that's what we called `Denial of Service`. In Smart Contract, it works just the same, if a function or operation in smart contract is compromised and cannot be accessed by others, that's a `Denial of Service (DoS)`. There are many ways to make a `DoS` in Smart contract, be it to render the contract function unable to be done, exhausting the contract out of gas, looping over unbounded arrays, and the list goes on.

## Test Objectives

- Ensure that functions in the smart contract doesn't introduce DoS Potential.
- Ensure access control is properly applied.

## How to Test



## References

- [Denial of Serice (DoS) Attack in Smart Contracts](https://www.nethermind.io/blog/denial-of-service-dos-attacks-in-smart-contracts#:~:text=Denial%20of%20Service%20(DoS)%20in%20the%20Web3%20realm&text=A%20DoS%20attack%20against%20a,intended%2C%20either%20temporarily%20or%20permanently.)
- [Common Vulnerabilities in Solidity: Denial of Service](https://www.slowmist.com/articles/solidity-security/Common-Vulnerabilities-in-Solidity-Denial-of-Service-DOS.html)
- [Vulnerability: Denial of Service](https://owasp.org/www-project-smart-contract-top-10/2023/en/src/SC06-denial-of-service-attacks.html)
- [Denial of Service (DoS) Attacks](https://www.quillaudits.com/blog/web3-security/denial-of-service-on-smart-contracts)
- [Denial of Service by Example](https://solidity-by-example.org/hacks/denial-of-service/)