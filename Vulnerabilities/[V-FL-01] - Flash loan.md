# Flash loan

## Summary

Flash Loan is something that cannot be bring to our real-world, since it introduce the possibility to take a loan, do something with the loan, and then payback the loan in a one single huge transaction. The existence of Flash Loan in DeFi has bring a good, bad, and worst seasons.

Flash Loans are susceptible to security riks, like price manipulation, oracle manipulation, and trigerrring a market volatility.

To explain this in a simple manner, let's say we have this scenario:
1. Attacker flash loaned 1000 ETH
2. Attacker buy 1000 Cryptocurrency worth of 1000 ETH on DEX A, which is `undervalued`
3. Attacker sell the 1000 Cryptocurrency on DEX B, which the tokens is `overvalued` at 1200 ETH
4. Attacker payback the 1000 ETH.
5. Attacke keeps the 200 ETH Profits.

This is a simple scenario taken from [Hacken - Simple Flash Loan](https://hacken.io/discover/flash-loan-attacks/)

## Test Objectives

- Ensure that calls interactions are secure.
- Ensure that proper access control mechanism is implemented.
- Ensure that only validated address can use the flash loan

## How to Test

To check for Flash Loans is actually quite tricky as it's now always available in all DeFi products. But when one does, we can always try to check for

1. **Reentrancy Guard**

Make sure that all external calls are protected and prevents for a multiple call to the contract after a callback.

2. **Whitelisting Appraoch**

Although it's depends on the devs, but white-lisiting approach is the best approach both in web2 and web3 for access control.

3. **Utilization of Various Oracle**

Utilizing more than 1 oracle to find the best and most accurate price is necesary to ensure fairness and preventing things like flash loans arbitrage.

## References

- [Hacken - Flash Loan](https://hacken.io/discover/flash-loan-attacks/)
- [Euler Finance Incident the $197 Million Heist](https://www.chainalysis.com/blog/euler-finance-flash-loan-attack/)