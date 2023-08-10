---
description: Synthesis of the xToken, veToken, and xChain native design
---

# Voting (vxPremia)

### Abstract <a href="#abstract" id="abstract"></a>

vxPREMIA is the voting token of the Premia Meta-Economy. The token represents a weighted balance of PREMIA tokens and utilizes a voting-escrow model popularized by Curve (veCRV) and custom sustainability mechanics. vxPREMIA is minted equal to your deposit of PREMIA tokens. This balance is then modified by your lock period, referred to as your “Boost”.

Your balance of vxPREMIA multiplied by your Boost modifier is your “Influence”. Wallet Influence is the key metric in determining your fee distribution, trading fee discounts, and votes for PREMIA token emission flow.

### Introduction <a href="#introduction" id="introduction"></a>

#### Background <a href="#background" id="background"></a>

The original design [xPREMIA](https://docs.premia.finance/archive/archive-deprecated/depr-premia-staking-earn-protocol-fees-w-xpremia), was elegant yet ever-evolving. It allowed for Premia deposits to receive a pro-rata share of protocol fee distributions, as well as the ability to reduce protocol fees paid by traders. Liquidity Mining (the Liquidity Provider incentive program) was controlled by Snapshot Governance, periodically re-allocating the directional flow of Premia Tokens.

The Premia Protocol has redefined the paradigm known as valve gauge weighting to introduce the vxPremia Reward Manifold.

**Why is it better than traditional ve?**

1. Utilization-based rewards mean that vote farming for low-volume pools is not economic and prevents DAO takeover of emissions (see: Balancer, Convex, etc.).
2. Omni-chain means that users can bridge their locked vxPREMIA to any chain supported by LayerZero and vote on emissions or earn rewards on whichever chain they currently see the most benefit.
3. Non-transferrable (soul-bound until unlocked), means that bribes are disabled by default, enabling healthier emissions governance.
4. Earn rewards in USDC, zap-compoundable into more vePREMIA at any time, means stakers earn highly liquid, stable yield.

#### Definitions <a href="#definitions" id="definitions"></a>

* **vxPREMIA:** Staked PREMIA tokens
* **Boost:** Modifier calculated by lock period length remaining
* **Influence:** Weighted PREMIA Balance
  * \=(vxPREMIA\*Boost)
* **Withdraw Delay:** 10-day delay once withdrawal requested
* **Early Unlock Penalty:** Penalty paid to remove the lock period
  * coefficient scales to lock time remaining - 25% per year left, max is 75%
  * feePercentage % fee to pay for early unstake (1e4 = 100%)
* **Liquidity Mining:** Option Pool Liquidity Provider Incentives in PREMIA Token
* **Reward Manifold:** Valve gauge control for emission flow of liquidity mining rewards
  * **Flowback Throttle:** Synthetic cap on applied influence based on utilization
    * \=(Influence \* max(utilization, 0.25))
    * This ensures each pool's rewards are based on the revenue produced by the pool
* **Universal Social Dividend:** Protocol Fee Distributions to vxPREMIA holders (USD Proceeds)
* **Rewards:** Protocol Fee Distributions (Social Dividend) & Liquidity Mining Incentives (Manifold)
* **Bridge:** Process to move a token from one chain to another (vxPremia currently uses [LayerZero](https://layerzero.network/))

### vxPremia Meta-Economy <a href="#usage" id="usage"></a>

#### Methodology <a href="#methodology" id="methodology"></a>

**Liquid Valves -** By locking Premia Tokens via the vxPremia Interface, users are now empowered to align the long-term platform vision with that of participants and citizens. Liquid Valve Gauge Weight is applied immediately and does not follow an epoch process. Each source chain has its corresponding manifold to distribute liquidity mining incentives.

**Protocol Commissions -** Pro-rata distributions of collected fees based on source chain utilization, collateralization, and settlement fees. Proceeds are applied based on the amount of influence on the source chain.

**Omnichain Gateway -** As now reward controls are at the chain level, there needs to be a fluid method to move vxPREMIA between chains. Utilizing our omnichain approach and LayerZero implementation, this is a quick and effortless transaction. The process is as follows: burn vxPREMIA on the source chain, mint vxPREMIA on the destination chain. In the LayerZero transaction, the boost modifier is automatically carried to the destination chain and combined with local vxPREMIA to update your size-weighted multiplier.

**Omnichain Hub Relay** \[In Development] **-** A method to sync cross-chain vxPREMIA Influence to direct Chain Weight of the Liquidity Mining Incentives from the Liquidity Mining Reservoir Fund (on mainnet) to the destination chains. This will complete the entire process of reward distribution, becoming decentralized, on-chain, and omni-chain.

#### **vxPremia Manifold Control**

Influence holders can direct their vote to any directional pool (call or put) on their vxPREMIA source chain. The total aggregate of the pool's valve gauge as a percentage of total influence directs that chain's share of liquidity mining incentives. Voting is liquid and applied immediately upon the next pool update transaction; there is no epochal update with Reward Manifold Control. To incentivize highly utilized pools and reduce the irrational application of influence, there is a mechanism known as the flow back throttle that creates a synthetic cap on the pressure applied at the valve level.

The **flow back throttle** is calculated as (Influence \* max(utilization, 0.25)); thus a less-utilized pool receives a haircut on votes applied. For example, if 1,000 influence is applied to pool "A" and the if utilization rate of pool "A" is 20%, then the influence applied to that pool is calculated as 250. If 1,000 influence is applied to pool "B" and the utilization rate of pool "B" is 85%, then the influence applied to that pool is calculated as 850.

#### **Premia Universal Social Dividend (USD Proceeds)**

**Distributed Protocol Fees**

\~50% distribution of accrued fees each month (decay function) is given to vxPREMIA holders via their pro-rata share of Influence. For example, if you own 20% of the chain's vxPREMIA influence, you will receive 20% of fee distributions.

#### **Fee Discounts**

* Externally owned accounts (ex: metamask wallet)
  * 60% Max Discount
  * 5,000 vePREMIA Influence = 10% Discount\
    50,000 vePREMIA Influence = 25% Discount\
    500,000 vePREMIA Influence = 35% Discount\
    2,500,000 vePREMIA Influence = 60% Discount
* Smart contract accounts
  * 30% Max Discount
  * Discount scales linearly from 0-30% as the contract's control of Total Influence on the Source Chain scales from 0-50%

#### Contract Functions <a href="#contract-functions" id="contract-functions"></a>

A contract allowing you to use your locked Premia as voting power for mining weights

1. **getPoolVotes -** Get total votes at the pool level
2. **getUserVotes -** Get total votes at the user level
3. **castVotes**
   1. Remove previously applied votes (if any)
   2. Apply new vote distribution
