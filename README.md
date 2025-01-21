# Solidity Smart Contract Testing Guide (S-SCTG)

Smol guide to audit a Solidity Smart Contract.

## The Idea

The Idea behind this **Solidity Smart Contract Testing Guide(S-SCTG)** is to provide a comprehensive and structured approach for testing smart contract, mainly inspired by [Smart Contract Security Verification Standart(SCSVS)](https://github.com/securing/SCSVS), [Immunefi Web3 Security Library](https://github.com/immunefi-team/Web3-Security-Library), DevCon 7 Bangkok Security Presentations and mediums.

While **SCSVS** is more suitable for the developers team, this testing frameworks will be focused on the security side- or designed to be used by **Smart Contract Auditor**. 

On the other hand, this guide hopes to give you a catalyst to further understand blockchain security.


### Field Guides
|  Topics | Link  |
|---|---|
| Blockchain Essentials | [Start reading](Blockchain%20Essentials/README.md#blockchain-essentials)|
| "Not so" Technical Stuff| [Start reading](#the-not-so-technical-stuffs)|
| Vulnerabilities | [Start reading](#vulerabilities) |

## The "Not so" Technical Stuffs
It's not that Technical, but this stuff actually bother either the Developer or the Auditor.

- [Design Patterns]([NTT-01]%20-%20Design%20Patterns.md#design-patterns) 
- [Gas Usage & Limitations]([NTT-02]%20-%20Gas%20Usage%20&%20Limitations.md#gas-usage--limitation)
- [Implementation of Standards]([NTT-03]%20-%20Implementation%20of%20Standards.md#implementation-of-standards)
- [Code Clarity]([NTT-04]%20-%20Code%20Clarity.md#code-clarity)
- [Test Coverage]([NTT-05]%20-%20Test%20Coverage.md#test-coverage)

# Vulerabilities
This is where "something bad about to happen" stuff, one miss and millions gone.

- Logic Error
    - General Logic Error
    - Uninitialized Components
- Arithmetic Vulnerabilities
    - [Integer Overflow/Underflow](Vulnerabilities/[V-AV-01]%20-%20Integer%20Overflow%20&%20Underflow.md#integer-overflow--underflow)
    - [Rounding Error](Vulnerabilities/[V-AV-02]%20-%20Rounding%20Error.md#rounding-error)
- [Reentrancy](Vulnerabilities/[V-RE-01]%20-%20Reentrancy.md#reentrancy)
    - [Single-Function Reentrancy](Vulnerabilities/[V-RE-02]%20-%20Single-Function%20Reentrancy.md#single-function-reentrancy)
    - [Cross-Function Reentrancy](Vulnerabilities/[V-RE-03]%20-%20Cross-Function%20Reentrancy.md#cross-function-reentrancy)
    - [Cross-Contract Reentrancy](Vulnerabilities/[V-RE-04]%20-%20Cross-Contract%20Reentrancy.md#cross-contract-reentrancy)
    - [Read-Only Reentrancy](Vulnerabilities/[V-RE-05]%20-%20Read-Only%20Reentrancy.md#read-only-reentrancy)
- Proxies, Upgradeable Smart Contracts
- Access Control Vulnerabilities
    - [Weak Identifier](Vulnerabilities/[V-AC-01]%20-%20Weak%20Identifier.md#weak-identifier)
    - Signature Verification
    - [Unprotected Functions](Vulnerabilities/[V-AC-03]%20-%20Unprotected%20Functions.md#unprotected-functions)
    - [Authentication with `tx.origin`](Vulnerabilities/[V-AC-04]%20-%20tx.origin%20Authentication.md#txorigin-authentication)
- [Standards Misimplementation](Vulnerabilities/[V-SMI-01]%20-%20Standards%20Misimplementation.md#standards-misimplementation)
- Flash loans
- Unchecked Return Value
- Insecure Randomness
- Denial of Service (DoS)
- Business Logic Error
- Using Components with Known Issues
