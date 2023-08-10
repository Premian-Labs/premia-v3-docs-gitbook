# Concepts

## Overview

The Premia ecosystem is designed to be an open, accessible, and innovative platform for decentralized finance. This overview provides a glimpse into the technical concepts and core functionalities that power the system. Whether you're a developer, trader, liquidity provider, or just curious about how it all works, this section is for you. Where possible, corresponding inputs and function calls are provided.  For a complete list of function calls available, please reference [contracts](broken-reference).&#x20;

### LP Range Orders

Range orders allow LPs to deposit over specific price ranges defined by lower and upper price bounds. [**More Details**](https://docs.premia.blue/the-premia-protocol/concepts/lp-range-orders)

* **Range Order Basics**: Requires three primary inputs: lower price bound, upper price bound, and size of Collateral or Option Contracts.
* **Depositing Collateral or Options**: Defines the exposure as either buy or sell, with specific ask-side and bid-side liquidity rules.

### Trading

There could be up to 4 sources of liquidity for a given option. [**More Details**](https://docs.premia.blue/the-premia-protocol/concepts/trading)

* **AMM**: Allows traders to receive quotes for buying or selling options.
* **Orderbook/RFQ System**: Enables makers and takers to provide and fill quotes.
* **Vaults**: Unique contracts that follow specific strategies for trading.
* **External Protocols**: Allows other protocols to utilize the pools as a settlement layer.

### Orderbook & Request-for-Quote (RFQ)

The Orderbook architecture in Premia is unique and utilizes two separate chains: Arbitrum One and Arbitrum Nova. [**More Details**](https://docs.premia.blue/the-premia-protocol/concepts/orderbook-and-request-for-quote-rfq)

* **Arbitrum One**: The primary chain where all orders are filled, canceled, and positions settled/exercised. All collateral and approvals must be on Arbitrum One.
* **Arbitrum Nova**: Used to reflect the order book on-chain, ensuring a decentralized token options exchange. It helps prevent front-running or censorship of orders.
* **Providing Quotes**: Quotes are emitted events of order details signed by the maker. They can be emitted:
  1. Directly through the IOrderbookStream interface on Arbitrum Nova.
  2. Using the Premia Orderbook API, Premia will publish the quote on the maker's behalf.
* **Access**: The order book can be accessed via the Premia Blue app, API, or SDK.

### Fees

The Premia V3 protocol has a structured fee system, including trading and pool initialization fees.  [**More Details**](https://docs.premia.blue/the-premia-protocol/concepts/fees)

* **Trading Fees**: Paid by traders taking liquidity (takers), split between LPs (makers) and protocol stakeholders. Calculated as the maximum of 3% of the total premium or 0.3% of the size. These fees can be claimed at any time.
* **Pool Initialization Fee**: Pool creation is permissionless and fee-based. The fee is dynamic and incentivizes users to initialize ATM pools (At The Money) and near-dated. Discounts are provided for specific expirations or strikes that create spread trading opportunities. Example: Initializing a 50 Delta option with 14 days expiration would cost $25.
* **Permissionless Pool Creation Bounds**: To prevent fragmentation, pools can only be created for common strikes and expirations. The maximum expiration is 365 days, with specific rules for expiration timing.
* **Oracle Support**: Premia v3 supports Chainlink oracles, Uniswap v3 pairs, or custom Oracle solutions.

### Exercise & Settlement

The exercise and settlement process within the Premia protocol is designed to be straightforward and flexible. [**More Details and Examples**](https://docs.premia.blue/the-premia-protocol/concepts/exercise-and-settlement)

* **Exercise**: Both call and put options can be exercised at any time after the option's maturity date. The payoff is locked to the intrinsic value if the option is In The Money at maturity.
* **Settlement**: Short option holders can claim the remaining collateral value after expiration. There's no penalty for late exercise or settlement.
* **Auto-Settlement**: Expired options can be exercised or settled by any approved user after expiration, with compensation for gas spent on settlement.
* **Settlement Price**: Determined via spot price oracles.

### Margin (Under Construction)

### Oracles (Under Construction)

### Liquidity Mining

Premia's liquidity mining is significantly changing, replacing traditional token liquidity mining with Premia Call Options. [**More Details**](https://docs.premia.blue/the-premia-protocol/concepts/liquidity-mining)

* **Discounted Call Options**: Liquidity providers will receive PREMIA Call options at a 45% discount to the underlying asset's current market price, replacing the existing liquidity mining scheme.
* **Proceeds Distribution**: If exercised, approximately 90% of the proceeds will circulate to vxPREMIA staking users, with the remaining 10% directed to Blue Descent (DAO).
* **Unexercised Options**: If not exercised, 20% of the option's intrinsic value will be locked for 1 year and then provided to the liquidity provider's wallet.
* **Fees**: Protocol Fees will be collected on exercise, and taker fees will be collected if the option is traded on the secondary market.
* **Modular Design**: Built modularly, allowing any market to be deployed as a physically settled option.  Premia encourages other protocols to reach out if they want to implement call options for their incentive programs.

### Advanced Exchange Concepts (Under Construction)

### Vaults / Depot

Premia Vaults, originally named "Meta-Vaults," are designed as passive liquidity vaults to enable users to use the current meta for options. They are meant to be used by traders and yield seekers to maximize returns and risk-management success in DeFi. [**More Details**](https://docs.premia.blue/the-premia-protocol/concepts/vaults)

**Premia v3 Vaults**

* **Margin Pool**: A Premia Vault created in-house to provide leverage to sell-side options on The Premia Protocol. It enables lenders to earn interest relative to their risk and option sellers to maintain lower capital requirements.
* **Other Vaults**: Includes the well-known C-Level Vault (Premia V2 Sell-Side Pools) and Knox Vaults, implementing option selling strategies for passive income. More composable Vaults, including Market-Making implementations, are planned for the future.

**3rd Party Vaults**

* **Permissionless Creation**: Anyone can create Vaults to deploy strategies utilizing the Premia Protocol.
* **Contributor Program**: A future program to showcase Third-party Vaults on the Premia Interface, benefiting both Premia and vault creators.

Premia Vaults are a key feature of the Premia ecosystem, offering diverse strategies and opportunities for in-house and third-party developers. They reflect financial markets' competitive and strategic nature, providing tools to optimize for specific environments or rulesets.
