# Containerized API

### <mark style="color:blue;">Overview</mark>

The containerized API is specifically for traders and institutions who would like to programmatically trade on Premia's orderbook. It includes all the features in the [Orderbook API](../orderbook-api/) (please read this section for better understanding of how the orderbook works) and abstracts away all other web3 functionality needed for _trading_.  Unlike the Orderbook API, which is hosted by Premia, the Containerized API is _self hosted_ by the individual trader or institution either on a local computer (individual) or cloud service (institution) of choice. &#x20;

{% hint style="success" %}
Premia does not manage or maintain any private keys for users of the Containerized API.&#x20;
{% endhint %}

### <mark style="color:blue;">Architecture</mark>

Premia is responsible for the maintenance of the repository for the Containerized API. This  repository can simply be cloned and stored either on a local computer of server.  It acts as the _middle man_ between the traders code and protocol interaction.  While the contents of the docker container were written in Typescript, this is not necessarily important to a trader.  Any language can be used to programmatically trade.

Even if a trader is not familiar with Typescript or Docker, the setup process is user friendly and saves considerable amount of development time.  Detailed instructions on the setup of the container can be found in the repository.

<figure><img src="../../../.gitbook/assets/architecture.png" alt=""><figcaption><p>Containerized API Architecture</p></figcaption></figure>

### <mark style="color:blue;">Setup Instructions</mark>

Detailed setup instructions for the Containerized API can be found directly in the repository README.  Once setup is complete, please review the [API Specification](api-specification.md) in the next section which provides a user friendly guide to accessing all the available endpoints.
