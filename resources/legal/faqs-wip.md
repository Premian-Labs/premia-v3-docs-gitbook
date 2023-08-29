# FAQs (WIP)

What is Layer 2?

OVERVIEW

To significantly scale trading, dYdX and StarkWare have built a Layer 2 protocol for cross-margined perpetuals, based on StarkWare’s StarkEx scalability engine and dYdX’s Perpetual smart contracts. Traders can expect significantly lower gas costs, and in turn, much lower trading fees and minimum trade sizes.\
\
StarkWare zkSTARKS technology is a form of ZK-Rollup technology that significantly increases dYdX’s trade settlement capacity, while still basing its security on the underlying Ethereum blockchain. StarkWare’s dYdX integration combines STARK proofs for data integrity with on-chain data availability to ensure a fully non-custodial protocol. Trades are settled on a Layer 2 system, which publishes zero-knowledge proofs periodically to an Ethereum smart contract in order to prove that state transitions within Layer 2 are valid.



Why is dYdX moving to Layer 2?OVERVIEW

Ethereum can process around 15 transactions per second (TPS), which is not enough to support the hypergrowth of DeFi, NFTs, and more. While Ethereum 2.0 will theoretically boost network speeds to 100,000 TPS, base layer scaling is still a while away. In the meantime, Layer 2 scaling solutions — in the forms of Rollups – free up Ethereum's base layer by offloading execution, leading to reduced gas costs and increased throughput without increasing network load. StarkWare’s dYdX integration combines STARK proofs for data integrity with on-chain data availability to ensure a fully non-custodial protocol.



Why choose StarkWare to build a Layer 2 solution?OVERVIEW

In order to address immediate scaling issues, dYdX's engineering team did extensive due diligence on various Layer 2 solutions and other Layer 1s.\
\
Overall, StarkWare was able to provide by far the best trading experience for our users in the shortest amount of time. Other Layer 1s do not yet have the collateral base and building blocks such as wallets and developer tools that have made Ethereum successful. While other Layer 2 solutions, such as optimistic rollups, are potentially promising, they are not as battle tested, don’t offer quite the same product experience (very long withdrawal times), and cannot offer the same level of decentralization & cryptographic guarantees as ZK-Rollups.\
\
Ultimately, we wanted a solution that could be live on mainnet within months, rather than an undefined period of time. StarkWare has already been running spot-trading exchanges in production, and has a stellar reputation in the industry around security and expertise.



Who is StarkWare?OVERVIEW

StarkWare develops software to improve blockchain scalability by allowing any type of computation to move off-chain, using the Ethereum blockchain as a public immutable commitment layer. Focusing on scalability, StarkWare has the fastest in-class technology for asserting computational integrity via succinct, transparent, and post-quantum-secure proofs.



How will dYdX change or benefit with Ethereum 2.0?OVERVIEW

Rollups today can offer over 100× scaling benefits without ETH 2. According to Vitalik Buterin, “if everyone moves to rollups, we will soon have \~3,000 TPS. Once ETH phase 1 comes along and rollups move to eth2 sharded chains for their data storage, we go up to a theoretical max of \~100,000 TPS. Eventually, phase 2 will come along, bringing eth2 sharded chains with native computations, which give us \~1,000-5,000 TPS.” While Ethereum 2.0 will theoretically boost network speeds to 100,000 TPS, base layer scaling is still a while away. There is increasing competition among various flavors in the rollup space with the trend being towards EVM compatibility and incorporating optionality for dApps depending on their needs



When will Layer 2 be live on mainnet?PRODUCT

We are launching a closed alpha for our cross-margined Perpetuals in mid-February. A full production launch will be available a few weeks later, once the operating status of the system has been thoroughly tested. Users can join our waitlist and will be notified when the full production is available.



Will there be more trading pairs in the future?PRODUCT

With cross margining and increased scalability, we will be able to launch many more markets on dYdX. During the alpha period, we will support Perpetuals for BTC-USD, ETH-USD, and LINK-USD, with many more pairs coming soon. We plan to launch 30+ new Perpetual Contracts over the course of 2022. We are focused on listing DeFi tokens, as well as the most traded cryptocurrency pairs by volume.



Which collateral will you accept?PRODUCT

All Perpetual Contracts will be settled and margined in USDC.



How do I deposit & withdraw from Layer 2?PRODUCT

For deposits, users can deposit funds to their account by sending a Layer 1 Ethereum transaction, specifying the amount, their position (vault) id, and their Stark Key through their wallet. Users can deposit USDC, or any other supported asset which can be converted to USDC on-chain via the 0x API. After the deposit transaction is mined, 10 Ethereum network confirmations (usually around 3 minutes) are required for your funds to be available to trade.\
\
Two types of withdrawals will be supported: **Slow** and **Fast** Withdrawals. Slow withdrawals must wait for a Layer 2 batch to be proved before they are processed. Layer 2 batches are proved roughly once every few hours, though this could be more or less frequent based on network conditions. Slow withdrawals occur in two steps: first the user requests a slow withdrawal, then once the next Layer 2 batch is proved the user must send a Layer 1 Ethereum transaction to claim their funds.



What are fast withdrawals?PRODUCT

Fast withdrawals utilize a withdrawal liquidity provider to send funds immediately and do not require users to wait for a Layer 2 batch to be proved. Users do not need to send any Layer 1 transactions to perform a fast withdrawal. Behind the scenes, the withdrawal liquidity provider will immediately send a transaction to Ethereum which, once mined, will send the user their funds. Users must pay a fee to the liquidity provider for fast withdrawals, and withdrawals are subject to a maximum size based on available liquidity.



How does cross-margining work?PRODUCT

Traders will be able to trade on multiple Perpetual Contracts using a single margin account, allowing for dramatically increased capital efficiency while trading multiple pairs. Collateral is held as USDC, and the quote asset for all perpetual markets is USD. Each market has two risk parameters, the initial margin fraction and the maintenance margin fraction, which determine the maximum leverage available within that market. These are used to calculate the value that must be held by an account in order to open or increase positions (in the case of initial margin) or avoid liquidation (in the case of maintenance margin)



How much leverage will be available?PRODUCT

We are now offering higher leverage – up to 25× – on dYdX for certain Perpetual Contracts. While some centralized exchanges offer over 100× leverage, most traders trade using low double digit leverage at most, and traders are prone to losing funds quickly at such high amounts of leverage.



How does trade settlement work?PRODUCT

Trades are matched off-chain and held in batches, until the proof of the validity of the batch is submitted on-chain, where it lives permanently. This prevents front-running of trade settlement and allows for instantaneous balance updates without waiting for a transaction to be mined. Trading on dYdX will feel every bit as fast as trading on a centralized exchange.



How will liquidations work, and what are the penalties?PRODUCT

Given the performance improvements of the oracles, we will be able to offer lower margin requirements, which means both higher maximum leverage as well as lower penalties when liquidated. Accounts whose total value falls below the maintenance margin requirement may have their positions automatically closed by the dYdX liquidation engine. Profits or losses from liquidations are taken on by the insurance fund.



Will Spot or Margin trading come to Layer 2?PRODUCT

dYdX is currently focused on derivatives trading. We will continue to support spot & margin trading, and will consider moving these to Layer 2 in the future.



Which price oracles will dYdX use?TECHNICAL

Asset prices on dYdX come from decentralized on-chain oracles such as Chainlink or MakerDAO, and are used to determine the collateralization of your account. These prices are fed to the dYdX smart contracts through price oracles that run on Ethereum.



Prices are attested-to by oracles using STARK-compatible signatures, allowing prices to be used as soon as they are signed, rather than waiting for a transaction to be mined. This significantly increases the economic security of the system against flash crashes, and allows for real-time liquidations.



What is on-chain vs. off-chain?TECHNICAL

To use the system, a trader first registers a Stark Key to their Ethereum Address in the StarkEx Contract. The StarkEx Contract contains a mapping from Ethereum addresses to Stark Keys, which is used as the off-chain identifier. This promises that if Alice withdraws money from an off-chain vault with a key that belongs to her, only she can actually control these funds on Layer 1. In addition, the on-chain contract handles registering, depositing and withdrawing USDC from/to the system, and also contains the commitment to the full balance state of the system, found in Layer 2.\
\
The off-chain logic handles trades, liquidations, transfers and deleverages, as well as updating the oracle prices. It stores the entire balance of all the users, and periodically submit STARK proofs, attesting the validity of the balances change, given the user’s transactions.



What is the impact of Layer 2 on liquidity & composability?TECHNICAL

If adoption becomes fragmented across a number of different Rollups in addition to liquidity on Layer 1 vs Layer 2, Ethereum's base layer is likely to lose some of the composability. However, it is likely that in the medium-term, new solutions will emerge to enhance composability.



Are my funds safe on Layer 2?SECURITY

In order to guarantee self custody of the funds, at any point in time, a user may opt to perform a forced request. Forced requests are initiated by a Layer 1 transaction, to avoid censorship. In case that the request is not served within a limited time frame, the user is able to freeze the StarkEx contract (and thus the exchange) and withdraw directly from the frozen contract. There are two forced actions: `forcedWithdrawal` and`forcedTrade`.\
\
When a `forcedWithdrawal` request is submitted, the StarkEx Contract either withdraws funds for you, or proves that the request is illegitimate (e.g. a user requests to withdraw more money than their off-chain position.) If StarkEx fails to withdraw funds within a limited timeframe, a user can call to the `freeze` function - effectively preventing the StarkEx contract from changing its state, and enabling an “escape mode”, in which users can exit with USDC based on the total value of their position in the last accepted on-chain batch.\
\
When a `forcedTrade` request is submitted, two users trade between them a certain amount of some synthetic against another amount of collateral, to submit these amounts on-chain. Either the system performs the trade (or proves its invalidity), or the users get the ability to freeze the on-chain StarkEx Contract, and retrieve their funds.



How does the insurance fund work?SECURITY

The insurance fund will not be decentralized at launch, and the dYdX team will be directly responsible for deposits & withdrawals to and from the fund. In the future, we may decentralize some aspects of the fund; however, initially, our priority is to ensure that underwater accounts are dealt with in a timely manner.



Are the new smart contracts audited?SECURITY

We take the security of our smart contracts extremely seriously. We’ve conducted rigorous internal testing and contracted independent top security firm PeckShield to perform a thorough audit of our smart contracts. Our audit will be open-source, and verifiable by anyone. Further, our track record speaks for itself: since launching our first product in October of 2018, dYdX is one of the only major DeFi protocols that has yet to receive a bug report of user funds being at-risk. No users have ever lost funds on dYdX.



What is dYdX's business model?COMPANY

dYdX has been building a business model around the exchange. We introduced maker taker trading fees in March 2020. Our goals have been to earn consistent revenue as a company, and to incentivize traders to provide more liquidity to our protocol.



What's next for the platform?COMPANY

After our Layer 2 launch, we will focus on decentralizing more parts of our technology stack and handing over more control to our users. This will be a gradual process that could ultimately result in a DAO to govern the community. We are taking a careful look at different design tradeoffs.



What is the future of decentralized exchange?COMPANY

While DEXs have become increasingly popular for spot products, we expect DEXs to become just as popular for margin and perpetual markets in the coming years. This is largely due to the fact that decentralized margin and perpetuals are newer and take time for users to adopt. Further, we think DEXs will continue to gain market share in relation to centralized exchanges, due to lower frictions to onboard, better UI/UX, increased security guarantees, and more attractive products to trade. dYdX combines the security and transparency of a DEX, with the speed and usability of a CEX. We think DeFi will supersede traditional finance technology only if the user experience & design are exceptional.
