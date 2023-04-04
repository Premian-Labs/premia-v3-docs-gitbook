# Trading

### <mark style="color:blue;">Overview</mark>

Long (short) options are represented as an [ERC-1155 token](https://eips.ethereum.org/EIPS/eip-1155) in a wallet. It allows any EOA or Contract address to easily transfer, trade, or exercise (settle) the option at a future time. When taking liquidity from any source, a transaction fee will need to be paid. More details on fees can be found [here](fees.md#trading-fees).

While all options settle within their respective pools, it’s possible to interact with _multiple_ sources of liquidity for the same option. Users of the [Premia Interface](../#the-premia-interface) or Premia v3 SDK will have easy access to the best quotes from each source. Please see here for more details about receiving a quote.

There could be up to 4 sources of liquidity for a given option:

1. AMM - Range Orders
2. Orderbook / RFQ System
3. Vaults
4. External Protocols / Third Parties

Interacting with the [AMM](trading.md#amm) and [Orderbook / RFQ](../the-premia-protocol/order-book-vs.-amm.md) System can be done directly via the `IPool` interface while [Vaults](vaults.md) and [External Protocols](trading.md#external-protocols-third-parties) would involve directly corresponding with their contracts or interfaces if not available on the Premia Interface or Premia v3 SDK.

### <mark style="color:blue;">AMM</mark>

Traders on the Premia v3 exchange - takers - can receive a quote to buy (sell) an option from LPs in an option pool at the given quote price using `getQuoteAMM`. If the taker deems the quote acceptable, they can `trade` the option and pay (receive) a premium in addition to receiving long (short) option contracts in the form of [ERC-1155 tokens](https://eips.ethereum.org/EIPS/eip-1155).

The trade function has 3 inputs: `size`, `isBuy`, and `premiumLimit`. The `size` parameter refers to the order size, `isBuy` is a boolean to signify trade direction, and `premiumLimit` is used for slippage control. If an order trades beyond the `premiumLimit`, ie. the average execution price of a buy (sell) order is above (below) the `premiumLimit`, the transaction will revert.

{% hint style="info" %}
The difference between a pool’s current `marketPrice` and `getQuoteAMM` in the `IPool` interface is the _price impact_ of a potential trade. This is a function of market liquidity and order size, where more a higher ratio of order size to existing liquidity will result in a higher price impact and vice versa.
{% endhint %}

It is possible to both pay and receive premiums and / or collateral for an option in the token of a user’s choice. This requires the user to define their swap parameters in the form of _calldata_, which will enable the swap to be executed on-chain before or after the necessary action to convert a token. The `swapAndTrade` and `tradeAndSwap` functions on each AMM pool can be used to facilitate this feature. More details can be found in the `IPool` interface within the Contract section.

### <mark style="color:blue;">Orderbook / RFQ System</mark>

There is no direct way to receive a _quote_ from the Orderbook via contract call. This must be done via the SDK, a custom indexer such as a Subgraph, or Third Party tooling. This is because the orders are not stored directly on-chain, rather, an on-chain event is emitted with the details of each order. This enables off-chain indexers to track the state of the Orderbook in a decentralized, trustless manner.

In addition to getting a quote, many other function calls can be executed via the `IPool` interface. The `fillQuoteRFQ` function allows a trader to fill an available quote from the Orderbook. When filling a quote, users will need to provide the corresponding `TradeQuote` struct, a `Signature` from the quote provider, and the designated `size` of the quote to fill.

Traders can verify if a quote is still valid in the Orderbook by calling `isQuoteRFQValid` using the same inputs as `fillQuoteRFQ`. RFQ Orders can also be canceled using `cancelQuotesRFQ` , by passing a list of quotes to cancel, or the fill status of a quote checked using `getQuoteRFQFilledAmount`.

Additionally, if there is a desire to do a swap before or after an RFQ trade, `fillQuoteRFQSwap` and `swapAndFillQuoteRFQ` can be used. More details can be found in the `IPool` interface within the Contract section.

### <mark style="color:blue;">Vaults</mark>

Each vault will have its own unique contract and functions, however, each must implement the `IVault` interface to be integrated in the [Premia Interface](../#the-premia-interface) and [Premia V3 ](#user-content-fn-1)[^1]SDK. If a third party vault correctly implements the `IVault` interface, it is eligible to be added to the V3 Vault Registry, which means its quotes will be automatically included in platform-wide quote streams.

Vaults need to implement the `getQuote` function, allowing users to request the vault’s price for a specific option (`strike`, `maturity`, `isCall`), including trade `size` and direction (`isBuy`). The vault should return both the `maxSize` and `price` for the trade, including a `maxSize` of 0 if the option is not offered by the vault. To enable quotes to be filled, vaults should implement the `trade` function with the same parameters as the `getQuote` function.

In addition to implementing the above functions, vaults will additionally need to emit an `UpdateQuotes()` event (with no parameters) any time the vault’s quotes change, in order for the Premia Interface and Premia v3 SDK to track quote changes.

Anyone interested in learning more about developing vaults on top of Premia v3, [Reach out](broken-reference) to build with us!

### <mark style="color:blue;">External Protocols / Third Parties</mark>

It is entirely possible for other protocols/users/vaults to utilize the pools strictly as a settlement layer and exchange options outside of Premia. To be able to do this, the `writeFrom` function can be used.

`writeFrom` is a method that will mint both the long and short option, and send each to the designated addresses for `underwriter` and `longReceiver`. In order to invoke this function, the collateral for the short position must be provided. Using the `writeFrom` function will incur a transaction fee equal to 0.3% of the notional value:

$$
Minting\:Fee = 0.003*Size*Collateral
$$

Collateral is 1 for calls and the strike price for puts. In other words, the minting fee here is denominated in the collateral of the option.

[^1]: 