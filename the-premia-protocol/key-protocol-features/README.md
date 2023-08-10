# Key Protocol Features

## <mark style="color:blue;">**Capital efficiency**</mark>

Liquidity providers are able to create concentrated range orders in an option pool with defined upper and lower price bounds. As traders move the market price through an LP’s price range, the LP will collect trading fees and the LP’s exposure will be updated. If the market price moves back down, the LP will collect more trading fees as the LP’s exposure will be reversed back to its original state.

A price range with more liquidity will require more option trades to be traversed than an equivalent price range with less liquidity, all else equal. This enables active traders with high conviction to benefit from highly capital efficient, concentrated orders, while passive traders with less conviction can still benefit from trading fees in a less capitally efficient range.

This increased capital efficiency can compounded by the use of a sell-side margin vault. The amount of collateral required to sell an option is often much larger than is required to buy the same option, thus causing an imbalance in option markets. A margin system introduced alongside the protocol enables sell-side LPs and option sellers (takers) to open positions on margin, up to an amount determined by an automated value-at-risk system.

## <mark style="color:blue;">**User functionality**</mark>

Traders, market-makers, and traditional LPs all receive additional functionality in this version of the protocol. Traders can buy or sell using market orders any time there is liquidity in the pool, with price impact determined by the size of the trade relative to the amount of liquidity in the effected price range(s).

LPs can place _uni-directional_ concentrated orders within a specific price range to express a position that will either get net-long or net-short as the market price traverses through the range.

Users with more passive intentions can utilize automated liquidity pools (vaults), built to manage specific trading, investment, and hedging strategies for the users within the vault’s liquidity pool.. The v3 system has been built as the base liquidity layer, on top of which many different strategies can be composed without liquidity fragmentation.

## <mark style="color:blue;">**Composability**</mark>

Diverse strategies can be created on top of the protocol, all benefiting from the more accurate price discovery, improved execution prices, and increased capital availability in the unified liquidity space. A wide range of vaults, traders, market-makers, and LPs can use the exchange for different reasons, without segregating and fragmenting liquidity for each use case. Due to the permissionless nature of the protocol, anyone can create a new market for an [ERC-20](https://eips.ethereum.org/EIPS/eip-20) pair, so long as each token has an on-chain spot price oracle (data feed). Options on the protocol are represented as [ERC-1155 tokens](https://eips.ethereum.org/EIPS/eip-1155) and LP positions are represented as [ERC-721 tokens](https://eips.ethereum.org/EIPS/eip-721), to preserve maximum composability.

For each trade in a pool, the exposure change and fee growth is distributed pro-rata across all liquidity overlapping the traded price range. Fees are accumulated separately and not compounded back into LP positions, enabling vaults to distribute fees according to their specific strategies. Pool capital can be borrowed for a fee with flash loans, increasing protocol and LP revenue generation. In addition, the historic pricing and volatility of options in each pool can be queried from the pool’s time-weighted average oracle, enabling deep integrations within other protocols.

## <mark style="color:blue;">**Protocol sustainability**</mark>

The Premia v3 protocol requires no maintenance.

Additionally, the governance of liquidity mining emissions is entirely on-chain with the introduction of the vxPREMIA system. This upgrade incentivizes stakers and LPs to more actively participate in reward distribution decisions, ensuring the pools with the highest expected fee returns receive the highest subsidies. Expired option positions can also be settled by any user of the protocol. This feature is self-financed by withholding collateral for the cost of EVM transaction fees (gas), enabling the feature to be permanently supported without external funding.

## <mark style="color:blue;">Incentivization</mark>

The protocol currently incentivizes Vaults to provide liquidity to the Premia Protocol with PREMIA liquidity mining rewards. The result is more trading volume, as takers receive better execution prices and have more liquidity to counter-trade. It follows that higher volumes will result in higher returns from fee distributions for stakeholders of the protocol.

LPs are additionally incentivized to stake the PREMIA they receive from liquidity mining rewards for vxPREMIA, to earn a higher portion of trading fees, pay less fees on any taker trades, and direct more rewards to the pairs in which they receive the highest fee return.

To complete the incentive circle, vxPREMIA stakers are incentivized to direct rewards to the vaults and pools that generate the most volume and highest fee return, as it will result in the highest return from fee distributions. All of these incentivizes work together to increase net volume and available liquidity, improve price efficiency, and enable higher returns for users of the platform.
