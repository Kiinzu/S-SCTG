# Test Coverage

## Summary

Project usually comes with many testcase to tests the functionality of the smart contracts within. The higher the `Coverage`, the better each function and functionality testing are tested.

## Test Objectives
- Ensure that test coverage is at least 80% or higher
- Ensure that test coverage covers all critical functions

## How to Test
The test for coverage is usually has been done by the developer since this information is required for almost all audit. 

We will take an example of [OddNiBank](https://github.com/Kiinzu/ODD-NI-bank) Mini Codebase audit here. In the readme of the codebase, the coverage is actually given and there 

```bash
forge coverage
[⠔] Compiling...
[⠊] Compiling 26 files with 0.8.28
[⠒] Solc 0.8.28 finished in 1.39s
Compiler run successful!
Analysing contracts...
Running tests...
| File                 | % Lines        | % Statements   | % Branches     | % Funcs       |
|----------------------|----------------|----------------|----------------|---------------|
| src/OddNiBank.sol    | 22.86% (8/35)  | 18.42% (7/38)  | 13.64% (3/22)  | 71.43% (5/7)  |
| src/OddNiBenefit.sol | 61.11% (11/18) | 54.55% (12/22) | 41.67% (10/24) | 66.67% (2/3)  |
| Total                | 35.85% (19/53) | 31.67% (19/60) | 28.26% (13/46) | 70.00% (7/10) |
```

The overall coverage of the codebase, especially the `functions` seems to be only at 70%, which is under the standards. The result of 70% actually shown in color as yellow, what we want is green color. With this situation, from the POV of the Auditor, they will have to look for another scenario that hasn't been tested or try to break some that has already been tested, thus might required extra job. From the Developer POV, it's actually a reckless move, since less test to be perform mean that the functionality that hasn't been tested might be compromissed since it has not been tested yet, it's like 50-50 chance of the function to be secure or the Auditor to find bugs there.

## Tools
- Foundry

## References
- [OddNiBank](https://github.com/Kiinzu/ODD-NI-bank)