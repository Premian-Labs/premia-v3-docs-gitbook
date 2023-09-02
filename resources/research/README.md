# Research

## Whitepapers

### **Premia Blue (Premia V3 Specification)**

[**Premia V3 Spec Whitepaper**](https://premia.blue/v3.pdf) Last Updated: Mar 24th, 2023

[**Premia V3 Spec Litepaper**](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FyZYDiwOkAUTz3OWc6dGF%2Fuploads%2FnaOYYZd07KWBGSijLHXp%2Flitepaper\_distribute.pdf?alt=media\&token=e43b7e07-4406-4491-862c-cf3b795ea952) Last Updated: June 29th, 2023&#x20;

**General Overview of V3**

1. **Exchange and Options**: The exchange supports buying and selling of call-and-put options, which are fully collateralized to minimize counterparty risk. Prices of call options are quoted in the underlying and prices of puts are quoted in the quote token (usually US-dollar stablecoins).
2. **Concentrated Liquidity and Range Orders**: Liquidity providers can create positions in specific option pools as concentrated range orders with defined lower and upper price bounds. Two distinct range orders, Collateral-Short (CS) and Long-Collateral (LC) are necessary to enable LPs to gain or reduce long or short exposure.
3. **Accounting and Fees**: The exchange uses a tick-based system to distribute the fee income collected pro-rata.
4. **Margin and Leverage**: Lending pools are introduced that allow LPs and LTs to borrow and partially collateralize their short option contracts. Borrowers pay an upfront commitment fee based on the utilization of the total available capital in the pool.
5. **OTC Liquidity**: The over-the-counter (OTC) quote system (RFQ) enables vaults and market-makers to provide quotes to users or aggregators on- or off-chain, increasing liquidity across strikes and maturities.

### **Depot/Vault Specifications**

[**Premia V3 Underwriter Vault Litepaper**](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FyZYDiwOkAUTz3OWc6dGF%2Fuploads%2FObM8YHOOU3SFa1q6lmPp%2Funderwritervault\_documentation.pdf?alt=media\&token=6f1c9fac-896a-471b-85b2-bc5c0cc7ff6c) Last Updated: April 6th, 2023&#x20;

This Options Vault strategy involves maintaining state variables and tracking option listings to manage user interactions and short options positions. It uses a pricing model that includes the fair value of the option, a vault-charged spread, and a minting fee, with spreads dispersed over the option's lifetime to prevent arbitrage. Lastly, it collects management and performance fees from liquidity providers and allows users to buy options, with trades passing specific filters and a spread charged via a C-level function.



**Archived Published Research**

[Premia V2 AMM](https://premia.finance/amm.pdf)

### <mark style="color:blue;">Calculations</mark>

See whitepaper Appendix
