# Fee on Transfer/Rebasing Tokens

## Summary

In the world of DeFi and Smart Contract, receiving and transfering Ether is a common practice. The more advance types of ERC-20 tokens are the **fee-on-trnasfer tokens** and the **rebase tokens**

#### Fee-on-Transfer (FoT) Tokens

This ERC-20 Tokens are tokens that deduct a fee whenever they are transferred. The sender will be dibeed full amount minus the fee while the recipient receives the intended ammount. The fee will then be used for various purposes, such as funding the development or operations or just reducing the token supply.

#### Rebase Token

Unlike another ERC-20 tokens, this advances ERC-20 Tokens the Rebase tokens won't give you a static balance, rather it will simultaneously decreasing the balance of all token holders. The main reason behind this is to maintain a certain ammount of target price by increasing or decreasing the total supply to response the market conditions.

## Test Objectives

- Ensure that balance calculation for Rebase Tokens and FoT Tokens for holders are correct.
- Ensure that there is no Slippage or Liquidity Issues.

## How to Test

To check for this vulnerability is actually quite simple, we first need to make sure what kind of ERC-20 we are dealing with and with that information we can then determine the approach that we can use to check the calculation. A few things to take note here is that there is some Issues that could happens because of these tokens type.

1. **Slippage and Liquidity Issues**

Normally this happens with `Automated Market Makers (AMMs)` like Uniswap and Sushiswap, where they assume that the tokens amount sent is equal the amount received- this is broken with the `Fee-on-Transfer Tokens`.

2. **Breaking the ERC-20 Assumptions**

Protocols that use normal ERC-20 Standrad usually depends on the `balanceOf` variable, but the thing is the `Fee-on-Transfer Tokens` break this assumption and rendering the smart contract to store the wrong balance.

3. **Incorrect Balances with Rebase Tokens**

With rebase tokens continously increasing and decreasing, a protocol that can't catch up with the update of the tokens owned by a user could potentially introduce

-    `Over-Redepmtion`; a condition where user can withdraw more token that what ones actually has. `
-   `Under-Collateralization`; a lending scenarios, whena  negative rebase could reduce collateral value.
-   `Inaccurate Rewards`; Staking protocol might distribute reward with the wrong calculation due to increasing or decreasing tokens that ones holds.

## References
- [Fee on Transfer & Rebase Tokens: ERC-20 Security Bug You Neeed to Know](https://medium.com/@0xnolo/fee-on-transfer-rebase-tokens-an-erc-20-security-bug-you-need-to-know-f4e5badea1ee)
- [Understanding the Risks: Logical Errors in Rebase Tokens](https://www.bulbapp.io/p/f531b91b-88c9-434e-8eb6-fb540aa23927/understanding-the-risks-logical-errors-in-rebase-tokens)
- [Ten issues with ERC20s that can ruin your Smart Contract](https://medium.com/@deliriusz/ten-issues-with-erc20s-that-can-ruin-you-smart-contract-6c06c44948e0)