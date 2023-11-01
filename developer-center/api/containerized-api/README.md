# Containerized API

### <mark style="color:blue;">Overview</mark>

The containerized API is specifically for traders and institutions who would like to programmatically trade on Premia's orderbook. It includes all the features in the [Orderbook API](../orderbook-api/) (please read this section for better understanding of how the orderbook works) and abstracts away all other web3 functionality needed for _trading_.  Unlike the Orderbook API, which is hosted by Premia, the Containerized API is self hosted by the individual trader or institution either on a local computer (individual) or cloud service (institution) of choice. &#x20;

{% hint style="info" %}
Since this API solely deals with the Orderbook, and functionality around it, is
{% endhint %}

### <mark style="color:blue;">Architecture</mark>

Premia maintains a repository for the Containerized API. This  repository can be cloned and stored either on a local computer of server.  It acts as the _middle man_ between the trader and protocol interaction.  While the contents of the docker container were written in Typescript, this is not necessarily important to a trader. &#x20;

Even if a trader is not familiar with Typescript or Docker, the setup process is very user friendly and saves considerable amount of development time for programmatic traders.  Detailed instructions on the setup of the container can be found in the repository.

<figure><img src="../../../.gitbook/assets/architecture.png" alt=""><figcaption><p>Containerized API Architecture</p></figcaption></figure>

### <mark style="color:blue;">Setup Instructions</mark>

Detailed setup instructions for the Containerized API can be found directly in the repository README.  Once setup, please review the [API Specification](api-specification.md) which provides a user friendly guide to accessing all the available endpoints.
