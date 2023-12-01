# Orderbook API

### <mark style="color:blue;">Overview</mark>

The Premia Orderbook API can be used to `Publish` and/or `Get` quotes from the orderbook and RFQ system for any deployed/deployable option pool.   The Orderbook API comprises of both a [REST API](rest-api.md) and a [WEBSOCKET](websocket.md).  There is no centralized dependency to `Publish` or `Get` quotes.  The orderbook API is simply a convenience wrapper around web3 calls so that others do not need to aggregate the data themselves.

{% hint style="success" %}
This API is primarily for the aggregation/display of decentralized orderbook data. Actions such as [_cancelling_](https://docs-solidity.premia.finance/contracts/pool/IPoolTrade.sol/interface.IPoolTrade.html#cancelquotesob), [_filling_](https://docs-solidity.premia.finance/contracts/pool/IPoolTrade.sol/interface.IPoolTrade.html#fillquoteob), or checking a quotes [fillable](https://docs-solidity.premia.finance/contracts/pool/IPoolTrade.sol/interface.IPoolTrade.html#getquoteobfilledamount) status is NOT done through this API. If you are a trader please read through this section and then reference our [Containerized API](../containerized-api/).&#x20;
{% endhint %}

### <mark style="color:blue;">API KEY</mark>

In order to use the API directly or through the SDK, an api key is required.  Currently, all api key requests must be made to <mark style="color:blue;">research@premia.finance</mark>.  Please use subject line: 'API KEY REQUEST' and an api key will be returned. &#x20;

### <mark style="color:blue;">Pool vs Orderbook</mark>

Since all trades must make their way on-chain in some way shape or form, the pool contract for a specific market can be viewed as the "gateway" for this. The pool contract currently supports dual functionality for both interacting with the AMM and interacting with the Orderbook. Functions which include `OB` in their name are designated for Orderbook functionality

### <mark style="color:blue;">Orderbook</mark>

As mentioned in the [Orderbook Concepts](../../../the-premia-protocol/concepts/orderbook-and-request-for-quote-rfq.md), quotes are published (by emitting events) to Arbitrum Nova and can be accessed by anyone listening to events on-chain. The Orderbook API will publish quotes for market makers _on their behalf_ and aggregate public order events to create a limit orderbook for any deployed/deployable pool.&#x20;

When posting an order, a `deadline` is provided in the quote object which specifies the duration a quote is valid for.  When a deadline is surpassed, the quote will expire without the need for action by the quote maker. However, if a user would like to prematurely void a quote before the deadline is hit, they must invoke the cancel function within the pool contract [here](https://docs-solidity.premia.finance/contracts/pool/IPoolTrade.sol/interface.IPoolTrade.html#cancelquotesob).&#x20;

To _fill_ an order that was published to the orderbook, a user must call the fill function available on the pool contract [here](https://docs-solidity.premia.finance/contracts/pool/IPoolTrade.sol/interface.IPoolTrade.html#fillquoteob). Filling orders (as a taker) will incur a transaction fee.  Please see [Fees](../../../the-premia-protocol/concepts/fees.md) for more details.&#x20;

{% hint style="warning" %}
IMPORTANT: Unlike LP's, makers who utilize the orderbook do NOT collect trading fees.
{% endhint %}

### <mark style="color:blue;">RFQ</mark>

Request-For-Quote is an additional feature available through the orderbook infrastructure in which users can publish _requests_ for a specific pool and receive _personalized_ quotes from market makers.  Publishing a request can only be done via [WEBSOCKET](websocket.md), but receiving requests can be done via [REST API](rest-api.md) or [WEBSOCKET](websocket.md).

_What is an RFQ request look like? It is simply an object with the following details:_

* <mark style="color:green;">PoolAddress</mark> -> the pool address (option market) the quote is for
* <mark style="color:green;">Side</mark> -> either a 'bid' or 'ask'
* <mark style="color:green;">ChainId</mark> -> the chain id (Arbitrum One Mainnet or Goerli Arbitrum Testnet)
* <mark style="color:green;">Size</mark> -> the quantity that is be requested
* <mark style="color:green;">Taker</mark> -> the address of the user requesting the quote

When a market maker receives this request (via WEBSOCKET), they will then generate a personalized quote and publish a quote to the orderbook for this request. _There is no guarantee that a quote will be returned back after publishing an RFQ request._

### <mark style="color:blue;">Orderbook vs. RFQ Quotes</mark>

The main difference between an RFQ quote and an orderbook quote is that the `takerAddress` is not populated within the quote object for standard quotes.  This mean the quote can be filled by any taker address on Arbitrum One.  This makes the quote "public".  If the `takerAddress` is populated with an address, it can only be filled by this address, making the order "private". Private quotes are created when a Request-For-Quote is made by a user.  See [here](websocket.md#publish-rfq-request-s) for more information on how to publish an RFQ.

{% hint style="success" %}
_<mark style="color:orange;">NOTE: It's important for market makers to publish RFQ quotes with the</mark> <mark style="color:orange;"></mark><mark style="color:orange;">`takerAddress`</mark> <mark style="color:orange;"></mark><mark style="color:orange;">in the quote object populated with the</mark> <mark style="color:orange;"></mark><mark style="color:orange;">`taker`</mark> <mark style="color:orange;"></mark><mark style="color:orange;">address from the request (as opposed to Zero Address).</mark>_ While the quote is viewable by anyone, the quote can only be filled by the requester.
{% endhint %}
