# Fee Schedule

## Exchange Layer Fees

{% hint style="info" %}
Maker: A “maker” order is an order that adds liquidity to the order book. A maker order is not executed instantly but is placed on the order book or AMM. \
Taker: A “taker” order is an order that removes liquidity from the order book. A taker order is an order that is executed instantly against other orders upon submission.
{% endhint %}

| Fee Category           | Premia  | Deribit         |
| ---------------------- | ------- | --------------- |
| Maker Fee              | 0.00%   | 0.03%           |
| Taker Fee (AMM)        | 0.30%\* | -               |
| Taker Fee (Orderbook)  | 0.08%   | 0.03%           |
| Settlement Fee (Short) | Free    | Free            |
| Excercise Fee (Long)   | 0.30%\* | 0.015%          |
| Quoting Fee            | Free    | -               |
| Deposit Fee            | -       | Free            |
| Withdrawal Fee         | -       | Variable        |
| Multi-Leg (Combo)      | TBD     | Highest Fee Leg |

For more information on calculations, please see [Concepts>Fees](../concepts/fees.md)

This is the current fee schedule on Premia V3 (active version) and is subject to change anytime.

\*This fee is capped at 12% of the Option's value.
