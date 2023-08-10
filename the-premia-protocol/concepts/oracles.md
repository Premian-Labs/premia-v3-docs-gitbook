# Oracles

## <mark style="color:blue;">Price Feed Oracles</mark>

### Price Feed Overview

In order to create an options market for a given token pair, a valid oracle must be available to determine the value of an option (based on spot price) at expiration. Premia v3 pools can be established out-of-the-box with [Chainlink VWAP Price feeds](oracles.md#chainlink) or [Uniswap v3 TWAP Price feeds](oracles.md#uniswap-v3). Additionally, any Price Oracle implementing the `IOracleAdapter` interface can be used to initialize a new option pool.

By default, options will be automatically settled by an address maintained by the protocol, but Option holders can at any time designate an `autoSettleAddress` and an `autoMaxSettleFee`. Once set, the `autoSettleAddress` can exercise or settle options for the user after expiration, and be given a credit to compensate for the gas fees associated with making the call.

For convenience a keeper bot is then used to hydrate each pool with its corresponding settlement price if the option has expired. However, this value can also be populated within each pool by _any_ user who calls the `exercise` or `settle` functions.

### Chainlink

Chainlink has many available feeds which can be found [here](https://docs.chain.link/data-feeds/price-feeds/addresses?network=arbitrum). If a direct pair is not available when a pair is upserted to the `ChainlinkAdapter`, an attempt will be made to combine multiple price paths to create a valid price feed.

To _initialize_ a pool, Chainlink is queried with the selected pair, and if a valid market exists, a VWAP price is returned to help determine the `initializationFee`.

#### Determining Settlement Price

Ideally, the settlement price of _any_ option would be determined by the spot price that corresponds exactly to 8AM UTC. If a price is _not available_ at this timestamp, there are several actions that are taken to determine the most appropriate price:

1. A valid price _nearest_ to 8AM UTC on the expiration date is queried. This may end up being _before_ 8AM UTC or _after_ 8AM UTC, whichever is closest.
2. If the last valid price is more than 25 hrs _before_ the expiration timestamp, it will revert. This will happen for up to 12 hrs _after_ the expiration timestamp.
3. If expiration has happened more than 12 hrs ago, the last price prior to the expiration is used regardless of the timestamp (it will be more than 25 hrs prior to expiration).

{% hint style="info" %}
&#x20;While it is not expected to be a common occurrence to delay settlement by up to 12 hours, price feed or liquidity issues can happen.
{% endhint %}

### Uniswap v3

Uniswap v3 pools can act like a time-weighted average price (TWAP) oracle for a given pair. This opens up the possibility for additional spot markets that may not be feasible with Chainlink oracles alone.

{% hint style="info" %}
Caution must be taken by users who choose to trade markets with Uniswap v3 TWAPS as there are [limited guardrails](https://blog.uniswap.org/uniswap-v3-oracles) to prevent market price manipulation in Uniswap pools for less liquidity spot markets.
{% endhint %}

To _initialize_ an option pool using the `IUniswapV3Adapter`, Uniswap is queried with the selected pair, and if a valid market exists, a 10-minute TWAP is returned to help determine the `initializationFee` that can be queried ahead of time in the `IPoolFactory` interface.

A time-weighted average price is provided through two functions, `quote` and `quoteFrom`, the TWAP period is by default 10 minutes. `quote` provides the TWAP using the latest observation. An observation is a recording of the pool state at a given block, it is updated after a swap is performed, if an observation for the current block has not been made. All pools must meet the required cardinality. This simply means that there must be enough observations to calculate a TWAP over a 10 minute period. `quoteFrom` adds an additional parameter, `target`. The `target` is the starting point from which the TWAP is calculated. For example, a target of 8:00AM UTC and a TWAP period of 10 minutes means that the TWAP is calculated from 7:50AM UTC to 8:00AM UTC.

The price for both functions is derived from aggregating the arithmetic mean tick and harmonic mean liquidity across active fee tiers, then calculate a weighted arithmetic mean tick. To learn more details about how Uniswap Price Oracles work, the [Uniswap v3 Handbook](https://uniswapv3book.com/docs/milestone5/price-oracle/) is a great place to start.

{% hint style="info" %}
It is also worth noting that Premia v3 option markets utilizing the `UniswapAdapter` require a Uniswap v3 pool on Arbitrum, since Arbitrum is the location of all option settlement for the protocol.
{% endhint %}

#### Determining Settlement Price

Premia v3 will utilize a 10 minute TWAP window to determine prices that originate from Uniswap v3. For options that expire at 8AM UTC, we will use the TWAP from 750AM UTC to 800AM UTC on the corresponding expiration date.

If a TWAP is _not available_ for this timestamp range, there are a couple actions that are taken to determine the most appropriate price:

1. A valid price _prior_ to 750AM UTC is returned by Uniswap.
2. If the oldest observation available is _after_ expiration, we will set the 10 min TWAP using the oldest observation.

### 3rd Party Oracles

Any price feed that implements the `IOracleAdapter` smart contract interface can be used to initialize a new pool. However, only well-established price feeds will be shown directly to users on the Premia Interface or using the Premia v3 SDK.

## <mark style="color:blue;">IV Oracle</mark>

<mark style="color:red;">\<UNFINISHED SECTION SEE NOTES BELOW></mark>

Pending a change to amberdata instead of Deribit

* ESSVI → model derived from Deribit data (subject to change)
* Used for → Underwriter Vault & Margin Vault
* Updates → every minute
