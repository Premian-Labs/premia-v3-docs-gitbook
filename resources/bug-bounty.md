# Bug Bounty

## Bug Bounty Program for Premia Finance <a href="#defibugbountyprogramforpremiafinance" id="defibugbountyprogramforpremiafinance"></a>

### Introduction <a href="#introduction" id="introduction"></a>

Premia Finance is a decentralized finance (Defi) protocol built on the EVM. As part of our commitment to ensuring the security of our platform, we have established a bug bounty program to encourage the community to report any potential vulnerabilities or issues they may find in our code; white mad hatters are also encouraged to apply.  The Premian Republic has deposited 100k USDC into the Hat's Finance Platform under the [Premia Bug Bounty Vault.](https://app.hats.finance/vaults)\
\
Community Members looking to add to the bounty amount can find out [more info here](https://docs.hats.finance/general/master) and [**here**](https://hatsfinance.medium.com/running-decentralized-and-community-oriented-bug-bounties-70605d769bbe)**.**

### Scope <a href="#scope" id="scope"></a>

The bug bounty program covers all smart contracts and code deployed on the Ethereum mainnet, Arbitrum, Optimism, and Fantom. The program's scope includes any potential vulnerabilities that could lead to the loss of user funds or compromise the integrity of the Premia Finance platform.

### Rewards <a href="#rewards" id="rewards"></a>

Rewards for valid bug reports will be distributed per the following guidelines:

* Critical: Up to 72% of the Bug Bounty Vault
* High: Up to 27% of the Bug Bounty Vault
* Medium: Up to 18% of the Bug Bounty Vault
* Low: Up to 3.6% of the Bug Bounty Vault

The determination of the severity of a bug report will be left to the discretion of the Premia Finance team as outlined by the [Hat's Finance Terms of Use](https://docs.hats.finance/general/terms-of-use-1).  100k USDC has been deposited into the vault at inception; however, there are plans to direct a portion of protocol fees to the vault, as well as community members, are encouraged to contribute to the bounty as well (deposits can be withdrawn at any time under 7d time delay as long as there is no active vulnerability report)

### Eligibility <a href="#eligibility" id="eligibility"></a>

Everyone except the Premia Finance team and their immediate family members are eligible to participate in the bug bounty program. By participating in the program, you agree to abide by the rules and guidelines outlined in this document.

### Rules and Guidelines <a href="#rulesandguidelines" id="rulesandguidelines"></a>

* Only previously undisclosed vulnerabilities will be eligible for rewards.
* The first reporter of a valid vulnerability will receive the reward.
* Determining the validity of a vulnerability and the severity of the report will be left to the discretion of the Premia Finance team.
* Duplicate reports of the same vulnerability will not be eligible for rewards.
* Vulnerabilities already reported by the Premia Finance team or another third-party security researcher will not be eligible for rewards.
* Attempting to exploit a vulnerability for personal gain will result in disqualification from the program and possible legal action.
* The Premia Finance team reserves the right to modify or terminate the bug bounty program without notice.

Known Areas where the concern is acknowledged; however, no action will be taken:

* V2 Contracts utilize the ChainLink function: latestAnswer(), which is no longer the recommended (latestRoundData) method to retrieve Oracle price.
* V2 Contracts do not utilize the ChainLink variable: latestTimestamp to validate that a stale price is not being utilized.
* LayerZero vxPremia sendFrom reverts due to out of gas on cross-chain transfers, resulting in storedPayload blockage.

### Reporting a Vulnerability <a href="#reportingavulnerability" id="reportingavulnerability"></a>

To report a vulnerability, please submit via the Hat's Anon Friendly Portal: [https://app.hats.finance/vulnerability](https://app.hats.finance/vulnerability)\
\
If you would like, you can email dev@premia.finance with the subject line "Bug Bounty Program Report". The email should include a detailed description of the vulnerability and steps to reproduce it. Please also include your Arbitrum wallet address for reward distribution. If submitting only via the Hat's vulnerability ticketing system, an email is unnecessary.

Once the report is received, the Premia Finance team will review the vulnerability and respond with a determination of its validity and severity. If the report is valid, the team will work to fix the vulnerability and distribute the reward to the reporter.

### Conclusion <a href="#conclusion" id="conclusion"></a>

The Premia Finance bug bounty program is an important part of our commitment to security and the protection of user funds. We encourage anyone with knowledge of potential vulnerabilities to participate in the program and help us ensure the integrity of our platform.

For Terms and Conditions please see [bug-bounty-terms.md](legal/bug-bounty-terms.md "mention")\


**About Hats Finance**\
Hats Finance is the first of its kind, community-owned, and decentralized bug bounty protocol. Because security exploits affect all parties involved, Hats Finance facilitates community involvement by allowing users to provide liquidity to their favorite bounties and earn $HAT tokens once they become available. We hope that by allowing community involvement, we can raise awareness of the importance of security in crypto while encouraging white-hat hacker involvement.  Learn more about the Hats mechanism [here](https://hatsfinance.medium.com/running-decentralized-and-community-oriented-bug-bounties-70605d769bbe) and deposit in Premia’s bug bounty through the [Hats dApp.](https://app.hats.finance/vaults)
