# Exercise & Settlement

When a user is long (or short) an option, they **can `exercise` (or** `settle`**)** their option from the `IPool` interface **at any time after the option’s maturity date**. If at the time of maturity, an option is [_In The Money_](https://www.investopedia.com/ask/answers/042715/what-difference-between-money-and-out-money.asp), **the payoff is locked to the intrinsic value and can be claimed at any time post-maturity by the owner of the long option. Short option holders can also claim the remaining value of collateral any time after expiration.** There is no penalty for late exercising or settlement.

{% hint style="info" %}
**Expired options can be exercised or settled by any approved user after expiration, if enabled by the option holder. The address settling the option will be compensated in return for the gas spent on settlement, up to the maximum fee set by the option holder in advance, in order to maintain self-sustaining auto-settlement.**
{% endhint %}

## <mark style="color:blue;">**Exercise/Settlement of a PUT option**</mark>

If at the time of maturity, the price of the underlying asset is lower than the breakeven price, a put option is considered **In The Money**. In this case, the option owner (long the option) is entitled to the payoff, which is equal to the strike price of the underlying minus the spot price. This difference is calculated automatically and settled **in cash** (the quote token) to the option buyer. The option underwriter (short the option) is charged against their collateral held.

<mark style="color:orange;">**Example:**</mark>

An user buys a 2 ETH/USDC put option, at a strike price of 3,000 USDC. At the time of exercise, the ETH price is 2,700 USDC. The user receives the difference between the strike price and the spot price (3,000 - 2,700), multiplied by the amount of options they own (2). In this case, the user receives 600 USDC on exercise. The counter party who is short the option must settle their position. They will 2,700 USDC for each option they were short (in which they provided 3,000 USDC as collateral).

## <mark style="color:blue;">**Exercise/Settlement of a CALL option**</mark>

If at the time of maturity, the price of the underlying asset is higher than the breakeven price, a call option is considered **In The Money**. In this case, the user is entitled to the payoff, which is equal to the strike price of the underlying minus the spot price. This difference is calculated automatically and settled **in the underlying asset** (the base token) to the option buyer.

<mark style="color:orange;">**Example:**</mark>

A user buys a 2 ETH/USDC call option, at a strike price of 3,500 USDC. At the time of exercise, the ETH price is 4,000 USDC. User receives the difference between the strike price and the spot price (4,000 - 3,500), multiplied by the amount of options they own (2). In this case, the user is entitled to 1,000 USDC, which is settled in ETH. Thus the user receives (1,000 / 4,000 ETH) = 0.25 ETH on exercise.

## <mark style="color:blue;">Settlement Price (Oracle Feed)</mark>

Determining the settlement price is done via spot price oracles. In order to find out how the spot price is determined, please see here.
