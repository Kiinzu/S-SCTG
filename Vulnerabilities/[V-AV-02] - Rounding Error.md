# Rounding Error

## Summary

Solidity does not have a native floating-point type like `float` or `double` in other languages, this mean without a proper fixed-point arithmetic or scalling, the code might be vulnerable to a rounding error since it will be truncated.

## Test Objectives

- Ensure Fixed-Point Arithmetic is used.
- Ensure Multiply before Division

## How to Test

Rounding Error is some times hard to catch, but the damage is quite big since it continuously making people lost small amount of money consistently, there are quick checks that we can do:

1. **Scalling up point / `decimals`**
In ERC-20, there is a variable called `decimals`, it helps to scale up the operation to minimize floating point, or in other smart contract without the standard, we can also use a scalling point to scale up the operation just like having a `decimals`. If a contract has either of this thing, we can next checks for the result of the operations to determine if another scalling up is needed.

2. **Multiply before Division**
Because there is no floating point in solidity, a multiplication must be done first do eliminate the possibility of floats.

## Examples
We are going to take an example on [GlacierCTF 2024 - drainme](https://kiinzu.github.io/posts/GlacierCTF-2024/#drainme), here is the vulnerable part of the contract

```solidity
function depositEth() public payable {
        uint256 value = msg.value;
        uint256 shares = 0;

        require(value > 0, "Value too small");

        if (totalShares == 0) {
            shares = value;
        }
        else {
            shares = totalShares * value / address(this).balance;
        }
        
        totalShares += shares;
        balances[msg.sender] += shares;
    }
```

The calculation there is already right, but can be improved since it still introduce a rounding error problems.

Let's say that one buy a share worth of `1 wei` or 1 shares, the `totalShares` now become `1`. The next person who buy `100 wei` or 100 worth of shares, will be done by the `else` calculation which is `shares = totalShares * value / address(this).balance`, the calculation will be like this

```
shares = 1 (totalShares current) * 100 / 101 (1 + 100 wei)
shares = 100 / 101
shares = 0.999.....
shares of the new buyer = 0 (?)
```

why zero? it's because solidity doesn't have any decimals points, thus making the shares of `0.999...` effectively zero due to rounding down. Rounding Error is always associated with a loss of funds, may it be small or a large sum of loss.

## References
- [Solidity Design Patterns: Multiply before Dividing](https://medium.com/@soliditydeveloper.com/solidity-design-patterns-multiply-before-dividing-407980646f7)
- [GlacierCTF 2024 - drainme](https://kiinzu.github.io/posts/GlacierCTF-2024/#drainme)
- [Precision loss in Airthmetic Operations](https://blog.solidityscan.com/precision-loss-in-arithmetic-operations-8729aea20be9)