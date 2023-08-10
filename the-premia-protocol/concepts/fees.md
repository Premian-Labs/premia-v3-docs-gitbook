# Fees

## <mark style="color:blue;">Trading Fees</mark>

Transaction fees are paid by traders taking liquidity (takers) and split between LPs (makers) and protocol stakeholders at a rate determined by governance. To retain higher composability, transaction fees are accumulated separately and not compounded back into LP positions.

The trading fee is calculated as follows:

$$
Trading\:Fee = max(0.03*Total\:Premium,\: 0.003*Size)
$$

$$
Total\:Premium = Option\:Price * Size
$$

{% hint style="info" %}
NOTE: Trading Fees are claimable at _any_ time by calling the `claim` function on the `IPool` interface.
{% endhint %}

## <mark style="color:blue;">Pool Initialization Fee</mark>

Pool creation is _permissionless\* **and fee based. The fee is dynamic and incentivizes users to initialize pools that are ATM and near-dated, as they are the cheapest to create. Additionally, discounts are provided for specific expirations and/or strikes that have not been initialized that would create spread trading opportunities. The formal math behind how this is determined can be found in Appendix A.2 of the**_ [Premia v3 whitepaper](https://premia.finance/v3.pdf)_**.**_

The fee for a pool initialization can be queried from the `IPoolFactory` interface using the initializationFee method. As a basic example, if a user wanted to initialize a 50 Delta option with 14 days to expiration, the fee to do so would be $25.

**NOTE: Permissionless Pool Creation Bounds**

In order to prevent excessive fragmentation and ensure maximum composability with other option markets, pools can only be created for common strikes and expirations.

<mark style="color:orange;">Strikes</mark> are algorithmically limited to common values (log-rounded). The formal math behind how this is determined can be found in Append A.1 of the [Premia v3 whitepaper](https://premia.finance/v3.pdf).

The maximum <mark style="color:orange;">expiration</mark> an option can be created with is 365 days out. Options will all expire at 8AM UTC, with all options over 7 days to maturity expiring on a Friday. All options past 30 days must expire on the last Friday of the month.

Additionally, a spot market needs a valid <mark style="color:orange;">oracle feed</mark> for the token pair (ie. ETH/USDC). Premia v3 has out-of-the-box support for any Chainlink oracle, Uniswap v3 pair, or custom oracle solution that implements the `IOracleAdapter` interface.
