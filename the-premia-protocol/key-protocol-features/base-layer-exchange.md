# Base Layer (Exchange)

The base layer of PREMIA V3 is a complete rework of the protocol, focusing on capital efficiency in the decentralized finance (DeFi) options exchange. Here's a summary of the key components:

1. **Concentrated Liquidity**: Allows liquidity providers (LPs) to create positions in specific option pools with defined price bounds. It enables both active and passive traders to maximize fee collection and capital efficiency.
2. **Partial Collateralization**: Introduces margin architecture, enabling partial collateralization of options for select payoffs like spread strategies. This facilitates greater liquidity and more efficiently-priced assets while maintaining solvency and eliminating counter-party risk. (Enabled at a later date - ETA Oct 23)
3. **Transaction Fees & Liquidity Mining**: Traders pay fees and split between LPs and protocol stakeholders. Users can stake PREMIA tokens to collect fees and direct liquidity mining rewards.
4. **Vaults & OTC Liquidity**: Addresses challenges with granular options markets.
5. **Modular and Layered Protocol**: Designed for maximum composability and upgradability on top of the primitive base exchange layer.
6. **European-style Options Markets**: Enables the creation of European-style options markets for any asset pair with an on-chain spot price oracle.
7. **Range Orders**: Supports different types of range orders, such as Sell-with-collateral, Buy-with-shorts, Buy-with-collateral, and Sell-with-longs, each with specific functionalities.
8. **Margin System**: Using a risk-based model to assess user positions, blends attributes from traditional Reg-T and Portfolio Margin systems. Lending markets are established to provide capital to option underwriters. (Enabled at a later date - ETA Nov 23)
