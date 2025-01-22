# Signature Verification

## Summary

Ethereum also use Cryptography algorithm to authenticate and authorized someone. The EVM features the `ecrecover` precompile, allowing for native signature validity checking and recovery. This function is used in the context of authorization and data validity checks. 

Signature-related attacks is quite uncommon nowadays since the use of audited libraries like the [Openzeppelin ECDSA](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/cryptography/ECDSA.sol).

## Test Objectives

- Ensure that signature verification follows the best practice.

## How to Test

There are some vulnerabilities that are introduced by the using of Signature as authentication and authorization.

1. **Missing Validation**

The root cause of this vulnerability is when `ecrecover` encounters erros and return an invalid address, yet the invalid address still pas the check and allows an attacker to submit the signature.

2. **Replay Attacks**

A replay attack occur when the a signature and the system using it hjas no deduplication mechanism, rendering the system to unable to invalidate used signature. To counter this, devs could jsut put a unique variable like nonce.

3. **Frontrunning**

Frontrunning is a common issue and happened when there is a transaction that's pending in the pool. An Attacker can then watch the transaction and try to find a valid signature which then he will pay a slightly more gas to make his transaction prioritized by the blockchain and render the original transaction invalid.

4. **Signature Malleability**

In ECDSA, there is a known vulnerability where as long we can get the `v`, `r`, `s` of the signature, we can create another valid signature using the `-s . mod n`, which `n` value can be get since Solidity Elliptic Curve use `secp256k1`.

## References

- [Signature-related Attacks](https://scsfg.io/hackers/signature-attacks/)
- [Frontrunning](https://cow.fi/learn/what-is-frontrunning)
- [Replay Atttacks by Chainlink](https://chain.link/education-hub/replay-attack#:~:text=A%20replay%20attack%20is%20when,are%20is%20of%20paramount%20importance.)
- [The Elliptic Curve Digital Signature Algorithm ECDSA](https://dev.to/yusuferdogan/the-elliptic-curve-digital-signature-algorithm-ecdsa-jng)
- [Ethereum's Elliptic Curve Digital Signature Algorithm (ECDSA)](https://fitsaleem.medium.com/ethereums-elliptic-curve-digital-signature-algorithm-ecdsa-88e1659f4879)