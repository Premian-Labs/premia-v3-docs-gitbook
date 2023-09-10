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

1. A valid price _nearest_ to 8AM UTC on the expiration date is queried. This may end up being at or _before_ 8AM UTC.
2. If the last price update is more than 25 hours before expiration, the pool will go into a holding state until a fresh price update is received. In the event the oracle is unavailable for an extended period, a price override may be applied via governance to release the pool from the holding state

### 3rd Party Oracles

Any price feed that implements the `IOracleAdapter` smart contract interface can be used to initialize a new pool. However, only well-established price feeds will be shown directly to users on the Premia Interface or using the Premia v3 SDK.

## <mark style="color:blue;">IV Oracle</mark>

<mark style="color:red;">\<UNFINISHED SECTION SEE NOTES BELOW></mark>

Pending a change to amberdata instead of Deribit

* ESSVI → model derived from Deribit data (subject to change)
* Used for → Underwriter Vault & Margin Vault
* Updates → every minute
