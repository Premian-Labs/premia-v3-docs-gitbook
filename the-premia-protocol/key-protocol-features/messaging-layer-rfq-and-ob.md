# Messaging Layer (RFQ & OB)

The Messaging Layer or Over-the-Counter (OTC) Quote System in PREMIA V3 is a vital component designed to enhance liquidity and efficiency in the options market. It provides multiple functionalities and opportunities for integrations such as:

1. **Liquidity Provision**: Enables vaults or market-makers to provide fillable option quotes to users, either on-chain or off-chain. This minimizes the fragmentation of liquidity across the exchange.
2. **Just-In-Time Liquidity**: Offers just-in-time liquidity mechanisms to vaults and market-makers, allowing for highly capital-efficient market-making for passive and active users.
3. **Integration with Vaults**: Specific vaults can use the OTC quote system to provide liquidity across the entire volatility surface, further decreasing liquidity fragmentation and increasing capital efficiency.
4. **Volatility-Based Pricing**: Professional traders can provide volatility-based quotes to takers without incurring EVM gas fees for managing on-chain liquidity positions.
5. **Enhanced Liquidity Across Strikes & Maturities**: Increases liquidity across different strikes and maturities, enabling professional market-makers to optimize both execution price and transaction fees.
6. **Flexibility for Off-Chain Quotes**: Market-makers can provide quotes to users or aggregators off-chain, which can then be fulfilled through the exchange, even composable with range orders.
7. **Programmatic Trading**: The Messaging layer is available via the Premia Blue app, Typescript [SDK](../../developer-center/sdk/), and [API](../../developer-center/apis/orderbook-api/) (Auth Key & Docker for self-hosting).  This allows developers to integrate Premia V3 into their trading systems and algorithmic strategies.  For more information, see the [Developer Center](broken-reference).

The OTC Quote System in PREMIA V3 bridges on-chain and off-chain activities, facilitating a more seamless and efficient trading experience. It is key in reducing liquidity fragmentation and enabling more sophisticated trading strategies.
