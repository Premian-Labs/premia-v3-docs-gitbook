# Orderbook & Request-for-Quote (RFQ)

## <mark style="color:blue;">Arbitrum One vs. Arbitrum Nova</mark>

The Orderbook architecture is unique in that _two_ separate chains are utilized. The primary chain is [Arbitrum One](https://arbitrum.io/). Just like the v3 protocol, it's the primary chain where all orders are filled, cancelled, and positions settled/exercised.   This functionality can be accessed using the `IPool` interface, which resides on Arbitrum One, or via the Premia Blue app. Users of the Orderbook must have a valid address on Arbitrum One. All collateral and approvals to transfer collateral must be on Arbitrum One.&#x20;

{% hint style="info" %}
Why Arbitrum Nova? To be a true decentralized tokens options exchange, Premia believes the order book should also be decentralized so that all parties can ensure their orders are not being front ran or censored. A centralized off-chain order book may be subject to unwanted behaviors; thus, Premia decided to use Arbitrum Nova due to its low cost and fast transaction speeds to reflect the order book on-chain.
{% endhint %}

Providing _quotes_ for the order book are done through [Arbitrum Nova](https://nova.arbitrum.io/).  Quotes are nothing more than emitted events of order details that are signed by the maker of a transaction that will ultimately process on Arbitrum One.   There are two primary ways to emit quotes to the order book:

1. Directly through the `IOrderbookStream` interface using `add`.  This contract resides on Arbitrum Nova and requires a maker to have an account with an ETH balance to pay for gas fees. &#x20;
2. Using the [Premia Orderbook API](../../developer-center/api/orderbook-api/),  Premia will publish the quote on Arbitrum Nova on the maker's behalf.   This is a rate-limited / tiered feature.

{% hint style="info" %}
These orders do **not** earn maker rebates or liquidity mining rewards, ensuring concentrated liquidity and vaults stay competitive long-term.
{% endhint %}

The order book can be accessed directly via the Premia Blue app, [API](../../developer-center/api/orderbook-api/), or the [SDK](broken-reference). &#x20;
