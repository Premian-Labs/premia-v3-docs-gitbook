# Guides

### <mark style="color:blue;">Quick Start</mark>

The **full SDK** is free to use and open-source, currently available in the following languages:

* **Typescript / Javascript**

> 💡 The full SDK can be used trustlessly to execute any action or query any data from the protocol.

To get started just install the latest stable version of the package with,

```bash
yarn install "@premia/v3-sdk-public"
```

Then use the following code to get started with the minimal setup,

```typescript
API_KEY = '<infura-api-key>'
PRIVATE_KEY = '<private-key>'

premia = await Premia.initialize({
    provider: `https://arbitrum-mainnet.infura.io/v3/${API_KEY}`,
    chainId: SupportedChainId.ARBITRUM,
    privateKey: PRIVATE_KEY
})
```

The Premia v3 SDK is meant to be an all-in-one solution, just instantiate the `Premia` object and you will have access to the following:

* [analytics](https://docs-sdk.premia.finance/classes/AnalyticsAPI.html), query analytics information from the subgraph
* [contracts](https://docs-sdk.premia.finance/classes/ContractAPI.html), find access to the `Contract` types for each of Premia's main contracts.
* [options](https://docs-sdk.premia.finance/classes/OptionAPI.html), an aggregator for different sources of liquidity.
* [orders](https://docs-sdk.premia.finance/classes/OrdersAPI.html), provide and fill quotes through the orderbook (requires an API key).
* [pools](https://docs-sdk.premia.finance/classes/PoolAPI.html), interact with all functionalities related to the option pools.
* [pricing](https://docs-sdk.premia.finance/classes/PricingAPI.html), compare prices of different quotes
* [tokens](https://docs-sdk.premia.finance/classes/TokenAPI.html), query information about tokens in relation to the pools from the subgraph
* [pairs](https://docs-sdk.premia.finance/classes/TokenPairAPI.html), query information about token pairs in relation to the pools from the subgraph
* [transactions](https://docs-sdk.premia.finance/classes/TransactionAPI.html), query information about generic/vault transactions made on the protocol via the subgraph
* [users](https://docs-sdk.premia.finance/classes/UserAPI.html), query user information via the subgraph
* [vaults](https://docs-sdk.premia.finance/classes/VaultAPI.html), interact with all functionalities related to the vaults.
* [vxPremia](https://docs-sdk.premia.finance/classes/VxPremiaAPI.html), interact with all functionalities related to the vxPremia such as vault votes, user stakes, stake histories, and voting histories.

### <mark style="color:blue;">Walkthrough</mark>

#### Making Trades

First, we want to initialize the SDK with the proper configuration

```typescript
API_KEY = '<infura-api-key>'
PREMIA_API_KEY = '<premia-api-key' // Needed for utilizing the orderbook
PRIVATE_KEY = '<private-key>'

premia = await Premia.initialize({
    provider: `https://arbitrum-mainnet.infura.io/v3/${API_KEY}`,
    chainId: SupportedChainId.ARBITRUM,
    privateKey: PRIVATE_KEY,
    apiKey: PREMIA_API_KEY
})
```

Then we need to query the subgraph to see what pools are deployed because this will dictate which listings we end up selecting to get a quote for.

```typescript
const WETH = Addresses[SupportedChainId.ARBITRUM].WETH

const pools = await premia.pools.getPools(WETH)
```

Now that we have a list of available pools, we can filter and sort through them to find out in particular which pools are closest to the strike, maturity, and option type we are aiming for. For the sake of the example, however, we are just going to pick the first pool to demonstrate how to obtain a quote.

```typescript
// Get token contract
const tokenAddress = pools[0].isCall ? pools[0].base.address : pools[0].quote.address
const token = await premia.contracts.getTokenContract(tokenAddress)

// Get quote
const poolAddress = pools[0].address
const quote = await premia.pools.quote(poolAddress, parseEther('1'), true)
```

Now that we have our quote and it seems like it fits our target price range, let's send the transaction and make the trade.

```typescript
// Approve ERC20 token for the premium
await token.approve(quote.approvalTarget, quote.approvalAmount)

// Perform trade on the pool
const tx_response = await premia.pools.trade(poolAddress, {
    size: quote.size,
    isBuy: quote.isBuy,
    premiumLimit: quote.approvalAmount,
    referrer: ZeroAddress
})
```

There you have it, we've made our first trade on Premia!

### <mark style="color:blue;">API Reference</mark>

Detailed API documentation is available [here](https://docs-sdk.premia.finance/).
