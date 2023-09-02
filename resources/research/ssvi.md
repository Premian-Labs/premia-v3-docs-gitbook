---
description: Underwriter Vault Pricing Model
---

# SSVI

#### **Specification on Pricing Crypto Options**

**Background / Overview**

Options are financial derivatives that grant the holder the right to buy or sell an underlying asset at a predetermined price during a specified timeframe. They offer flexibility, strategic advantages, and hedging capabilities. Understanding the importance and mechanics of option pricing models is crucial for anyone diving into the complex world of options trading, especially in crypto.

**Importance of Option Pricing Models**

* **Fair Value Estimation**: This helps estimate the fair value of an option, allowing investors to identify mispriced options and exploit arbitrage opportunities.
* **Risk Management**: Quantifies and manages risk linked to options, offering insights into how option prices respond to factors like asset price, volatility, and expiration period.
* **Option Strategy Evaluation**: Assesses a range of trading strategies, enabling the evaluation of risk-return profiles, profit potential, and breakeven points.
* **Market Volatility Assessment**: Allows investors to assess implied volatility levels, offering insights into market expectations and sentiment.

**Historical Context**

* **Black-Scholes Model**: Revolutionized option pricing by providing a closed-form solution for European options. It considers asset price, time to expiration, interest rates, and volatility.

**Popular Models**

* **Black-Scholes Model**: Widely used, assumes geometric Brownian motion and efficient markets. Has limitations like constant volatility assumption.
* **Binomial Option Pricing Model**: A discrete-time model considering a series of time steps until expiration. More flexible than Black-Scholes but computationally intensive.
* **Other Models**: The Heston Model introduces stochastic volatility. Monte Carlo Simulation and Lattice Models offer more flexibility and accuracy but are computationally intensive.

**Key Inputs**

* Current asset price, option’s strike price, time to expiration, risk-free interest rate, and volatility are the key inputs in most models.

**Key Models**

1. **SVCJ Model**: High volatility and frequent price jumps in crypto make this model a fit.
2. **BR Model**: Adapts to sudden jumps in both returns and variance processes.
3. **eSSVI Model**: Builds on SVI but offers a more arbitrage-free surface. Uses a vega-weighted objective function for calibration.
4. **Global eSSVI Model**: Offers a global and arbitrage-free parametrization. Faster and ensures an arbitrage-free fit of market data.
5. **SVI Model with Parametrization**: Focuses on arbitrage conditions and offers two methods for fitting the smile: slice-to-slice and xSSVI fit. Also discusses weights, interpolation, and extrapolation techniques.
6. **KTH SVI Model**: A technical deep dive into SVI parametrizations, including Kellerer's theorem and implications for implied volatility. Strong focus on parameter calibration using the Nelder-Mead method.
7. **Local-Stochastic Volatility (LSV) Model**: Introduced as an alternative to LV and SV models. Captures the dynamics of the implied volatility surface and provides more accurate pricing for exotic path-dependent derivatives like ABRCs.

**Market Dynamics**

* **High Volatility**: Due to crypto's volatility, traditional models often fall short.
* **Lack of Regulation**: No central body complicates pricing.

**Econometric Properties**

* **Jump Locations and Frequencies**: Interpretable via stochastic volatility models with jumps.

**Trading Platforms & Risks**

* **Counterparty Risks**: High speculative patterns introduce risks.
* **OTC Market**: Big volume traders often go OTC, introducing its own set of risks.

**Regulatory Aspects**

* **Derivative Markets**: Platforms like CME aim to bring leverage and transparency.

**Practical Implications**

* **Hedge Against Volatility**: Properly priced crypto options can serve as a hedge.

**Data Filtering and Calibration**

* **eSSVI**: Uses vega-weighted objective function and sets specific data filtering rules for various exchanges.
* **Global eSSVI**: Introduces a new calibration algorithm that is not performed sequentially but globally on all slices. Effective in liquid markets but its effectiveness in illiquid markets remains a question.
* **SVI Parametrization**: Discusses arbitrage conditions and offers methods for fitting the smile. Also covers weights, interpolation, and extrapolation techniques.
* **KTH SVI Model**: Focuses on technical aspects of SVI parametrizations, including optimization methods like Nelder-Mead.
* **LSV Model**: Focuses on capturing the dynamics of the implied volatility surface and provides more accurate pricing for exotic path-dependent derivatives like ABRCs.

**Data Sources for Modeling and Interpolation**

To ensure the robustness and accuracy of our pricing models, we'll be utilizing a hybrid data approach:

* **Deribit Crypto Options Data**: Known for its comprehensive options market data, Deribit will be a primary data source for modeling the implied volatility surface.
* **Amberdata Crypto Options Data**: Renowned for its depth and breadth of crypto market data, Amberdata will complement Deribit's data to provide a more holistic view of the market conditions.

By leveraging these two data sources, we aim to model and interpolate the implied volatility surface with high precision, thereby enhancing the reliability of our DeFi options pricing model.

**Conclusion: The Superiority of eSSVI Models**

In the ever-evolving landscape of crypto options, the eSSVI model stands out as the superior pricing model for several compelling reasons:

* **Arbitrage-Free Surface**: One of the most significant advantages of eSSVI is its ability to offer an arbitrage-free implied volatility surface, a critical factor in the fast-paced, decentralized crypto markets.
* **Vega-Weighted Calibration**: The model's use of a vega-weighted objective function for calibration ensures that it is sensitive to market movements, making it highly adaptable.
* **Global Parametrization**: Unlike traditional models that require slice-to-slice calibration, eSSVI introduces a global parametrization, making it faster and more efficient.
* **Flexibility and Adaptability**: eSSVI is designed to be flexible enough to adapt to the unique volatility structures often seen in crypto markets, making it more reliable for pricing a wide range of options.
* **Market-Tested**: Given its robustness and adaptability, eSSVI has proven effective in liquid and illiquid market conditions, making it a go-to choice for traders and market makers.

In summary, the eSSVI model's blend of flexibility, efficiency, and market sensitivity makes it the superior choice for pricing options in the decentralized finance space.

**Sources**

* [**Pricing Cryptocurrency Options**](http://su.diva-portal.org/smash/get/diva2:1687028/FULLTEXT01)
* [**Pricing Cryptocurrency Options: The Case of Bitcoin and CRIX**](https://www.econstor.eu/bitstream/10419/230715/1/irtg1792dp2018-004.pdf)
* [**eSSVI Implied Volatility Surface**](https://assets.ctfassets.net/lmz2w5z92b9u/2dxRCEEtmhqW8eEOo3VX0C/fe78e006fa2187fc0193d4d91b644c3f/eSSVI\_Implied\_Volatility\_WP\_FY20.pdf)
* [**No arbitrage global parametrization for the eSSVI volatility surface**](https://arxiv.org/pdf/2204.00312.pdf)
* [**Royal Institute of Technology Paper**](https://www.diva-portal.org/smash/get/diva2:1348300/FULLTEXT02.pdf)
* [**KTH Royal Institute of Technology Paper**](https://kth.diva-portal.org/smash/get/diva2:744907/FULLTEXT02.pdf)
* [**Local-Stochastic Volatility (LSV) Model**](https://www.aimsciences.org/article/doi/10.3934/fmf.2022008?viewType=HTML)
