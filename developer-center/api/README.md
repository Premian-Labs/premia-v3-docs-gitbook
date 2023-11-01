---
description: Premia's API Suite
---

# API

There are several ways to programmatically interact with [The Premia Protocol](../../#the-premia-protocol).  In this section we will specifically be talking about access via an API.  Currently we have 3 major APIs set up.  Please take note which ones are more applicable for developers, and which ones are more applicable for traders.

### [Orderbook API](./#orderbook-api)

Direct use of the Orderbook API is suggested for _advanced_ web3 developers (ie aggregator protocols), not traders.  For a more trader centric API, please have a look at our [Containerized API](./#containerized-api) solution.

The orderbook API is useful for _direct_ access to the orderbook and its current state.  It will require additional knowledge of web3 functionality and formatting as there are many aspects of the lifecycle of option trading that will require direct interaction with our on-chain smart contracts.  &#x20;

### [Subgraph API](./#subgraph-api)

Like the orderbook API, this is for advanced web3 developers.  Advanced data querying can be done via our Subgraph API.  This is useful for data science, dashboards, and other data centric projects. &#x20;

### [Containerized API](./#containerized-api)

The containerized API is an "all-in-one" solution for users who would like to programmatically trade (in any coding language) using the orderbook, but do not want to be slowed down with "Defi integration" (aka web3).  This API solution is very similar to using a centralized exchange API, but with all the benefits of interacting with a decentralized exchange.&#x20;
