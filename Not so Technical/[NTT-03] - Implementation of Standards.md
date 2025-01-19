# Implementation of Standards

## Summary

Smart Contract Project can't go that far from Standards to make their project complete, this include **Ethereum Improvement Proposal (EIP)** and **Ethereum Request for Comment (ERC)**.

## Test Objectives
- Ensure that Standards that being used is audited and secure.
- Ensure that deviation from standards doesn't introduce security risks.

## How to Test

Testing for standard in a `Not-That-Technical` is not that hard, just read the Standards that being used by the contract and ensure that the usage of the Standards didn't introduce a vulnerability, well most of the time, it won't, but be very careful when the word `override` is being used on the inherited functions from a Standard. There is where most of the bug lies.

## References
- [Zellic - Exploring ERC-4626: A Security Primer](https://www.zellic.io/blog/exploring-erc-4626)