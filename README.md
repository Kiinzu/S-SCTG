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

## Vulerabilities
This is where "something bad about to happen" stuff, one miss and millions gone.

- Logic Error
    - General Logic Error
    - Unintialized Variables / Components
- Arithmetic Vulnerabilities
    - Integer Overflow/Underflow
    - Rounding Error
- Reentrancy
    - Single-Function Reentrancy
    - Cross-Function Reentrancy
    - Cross-Contract Reentrancy
    - Read-Only Reentrancy
- Uninitialized Proxy
- Access Control Vulnerabilities
    - Weak Identifier
    - Signature Verification
    - Unprotected Functions
    - Authentication with `tx.origin`
    - Reuse of `msg.value`
- Wrong Implementation of Standards
- Flashloans
- Unchecked Return Value
- Insecure Randomness
- Denial of Service (DoS)
- Business Logic Error
- Using Components with Known Issues
