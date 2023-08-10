# Underwriter Depot

## _Introduction_

_The purpose of creating the underwriter vault is to migrate users from Premia V2 to Premia V3. The UnderwriterVault satisfies the ERC4626 vault standard and enables cross protocol compatibility._

## General Overview and Functionality

{% hint style="info" %}
_The vault's purpose is to cluster liquidity that will be used to underwrite options at prices close to Deribit's. The depositor's liquidity will be used to underwrite call and put options for a wide set of strikes and maturities._
{% endhint %}

### Depot Summary

1. **ERC4626 State Variables**: The vault maintains state variables like total assets and total supply. These are updated based on user interactions like deposits, withdrawals, mints, and redemptions.
2. **Tracking Option Listings**: The vault keeps track of its short option positions, i.e., strike and maturity combinations. This uses variables like maturities, maturityToStrikes, minMaturity, and maxMaturity.
3. **Accounting of Locked Spreads**: The price quoted by the vault includes the fair value of the option, the spread charged by the vault, and the minting fee paid to the pool. Spreads collected from underwriting options are considered profits and are dispersed linearly over the option's lifetime to prevent front-running trades.
4. **Fee Collection**: LPs pay management and performance fees. Management fees are for managing LPs' assets, and performance fees are for generating positive returns. Fees are triggered upon a transfer or redemption and are only paid on the transferred/redeemed amount.
5. **Trade**: Users can buy call/put options from a vault, given that a pool with the corresponding maturity, strike, and option type exists. Trades need to pass filters based on the option's delta and the days to expiry. The vault charges a spread using the c-level function.

_User Stories_

_• As a depositor, I want to deposit collateral into the vault to receive shares that can later be redeemed for the premium and spread that is made by selling options to buyers in addition to my original deposit._

_• As a depositor, I want to be able to withdraw from a vault when there is capital available so that I can recover my collateral and realize the pro-rata P\&L from the time spent in the vault._

_• As a buyer, I want to receive a quote for an option purchase so that I can know in advance what the premium will be for purchasing the options and compare the cost with other outlets._

_• As a buyer, I want to purchase options from the vault by paying a premium for long contracts._

_• As a vault operator, I want to settle all options until the current time so that I increase the vault's liquidity / available assets, such that it will be available for depositors to withdraw or buyers to purchase new options._

{% hint style="info" %}
This is just a summary of the Underwriter Options Depot - The full detailed explanation can be found within the [research specification](../../../resources/research.md)
{% endhint %}
