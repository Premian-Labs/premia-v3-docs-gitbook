# Depot Layer (Vaults)

The Vault Layer in PREMIA V3 plays a crucial role in enhancing liquidity and capital efficiency within the options market. Here's a brief feature summary:

1. **Vaults for Passive Users**: Vaults enable passive users to participate in options markets automatically, market-making, and utilizing yield capture strategies created by Premia contributors and third parties. They can utilize standard range orders or the quote-based liquidity system to execute strategies, splitting returns across liquidity within the vault.
2. **OTC Liquidity & Market-Makers**: The over-the-counter (OTC) quote system allows vaults and market-makers to provide fillable option quotes to users, minimizing liquidity fragmentation. This system increases liquidity across strikes and maturities and enables professional market makers to optimize for both execution price and transaction fees.
3. **Liquidity Across Volatility Surface**: Specific vaults can provide liquidity across the entire volatility surface, decreasing fragmentation and increasing capital efficiency. Third-party providers can create vaults, enabling anyone to participate in strategy discovery and monetization. This enables a many-to-one relationship between vaults and pools.
4. **Integration with Base Exchange Layer**: The vault layer is built into the base exchange layer, addressing challenges with liquidity fragmentation across strikes and maturities. It provides just-in-time liquidity mechanisms to vaults and market-makers, enabling highly capital-efficient market-making for passive and active users.
5. **Volatility-Based Pricing**: Institutional traders can provide volatility-based quotes without paying EVM gas fees to manage on-chain liquidity positions.

The Vault Layer's design aims to simplify the complexity of pricing and strategy creation within option markets, making it more accessible to passive users while optimizing liquidity and capital efficiency. It's a key component in PREMIA V3's architecture, contributing to a more functional and efficient decentralized token options exchange.
