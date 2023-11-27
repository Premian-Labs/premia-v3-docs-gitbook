---
description: Rest API End Points
---

# REST API

### <mark style="color:blue;">Overview</mark>

| Chain                    | Base Url                              |
| ------------------------ | ------------------------------------- |
| Arbitrum Goerli (421613) | https://test.orderbook.premia.finance |
| Arbitrum One (42161)     | https://orderbook.premia.finance      |

### <mark style="color:blue;">Publish Quotes</mark>

To submit a quote to the orderbook, `POST` quotes can be used.  In the background, we will take this quote an submit an event on Arbitrum Nova on the users behalf.  One the quote event is generated on Arbitrum Nova, it will be added to the orderbook.

{% swagger src="../../../.gitbook/assets/openapi3_0.yaml" path="/quotes" method="post" %}
[openapi3_0.yaml](../../../.gitbook/assets/openapi3_0.yaml)
{% endswagger %}

{% tabs %}
{% tab title="Publish Quote Example" %}
```javascript
// Javascript Sample Code
const { fetch } = require ('cross-fetch')
const { parseEther } = require("ethers");

const url = 'https://test.orderbook.premia.finance/quotes'

// Free or Paid Tier API KEY
const MOCK_API_KEY = '3423ee2bfd89491f82b351404ea8c3b7'

// object array of one or more quotes
const mockQuotes = [{
  poolKey: {
    base: '0x9f5D1212514Ac88E26c523387C60F87B67AD1130',
    quote: '0x53421DB1f41368E028A4239954feB5033C7B3729',
    oracleAdapter: '0xB89756634Bae979d13721d9883e67CF22cc31D6b',
    strike: parseEther('2900').toString(),
    maturity: 1688716800,
    isCallPool: false
  },
  chainId: '421613'
  provider: '0x9E600587b9035a8C1254E8256F4E588CC33B8467',
  taker: '0x0000000000000000000000000000000000000000',
  price: parseEther('.195').toString(),
  size: parseEther('1').toString(),
  isBuy: false,
  deadline: 1686592096,
  salt: 1686584896,
  signature: {
    r: '0x7feff79a77a023fd8e034066c5bdb78e9b9e5e3804dfc02c0d5b3979b5be8324',
    s: '0x58229484d69566752b48c49326e6eaca2b4fba5372ba638870bda5b58b4fb25e',
    v: 28
  }
}]


async publishQuote(quotes) {
  const response = await fetch(url, {
      method: 'POST',
      headers: {
          'Content-Type': 'application/json',
          'x-apikey': MOCK_API_KEY,
      },
      body: JSON.stringify(quotes),
  })
  
  if (!response.ok) {
    console.error('Request failed: ', await response.json())
    throw new Error(`Failed to publish quotes: ${response.statusText}`)
  }
		
  return response.json()
}


publishQuote(mockQuotes)
```


{% endtab %}
{% endtabs %}

### <mark style="color:blue;">Get Quotes</mark>

When using `GET` quotes, orders are returned based on the params details provided.  `quotes` endpoint is ideal for determining which order(s) will satisfy a specific order `size` and `side`. The returned quotes are ordered by price, and only the order(s) that satisfy the `size` (cumulatively) will be returned.&#x20;

Additionally, it is possible to receive RFQ quotes  along with public quotes by specifying a `taker` address.  The allows users to find the best possible public/private quote combination for a particular quote size.

{% swagger src="../../../.gitbook/assets/openapi3_0.yaml" path="/quotes" method="get" %}
[openapi3_0.yaml](../../../.gitbook/assets/openapi3_0.yaml)
{% endswagger %}

{% tabs %}
{% tab title="Get Quotes Example" %}
<pre class="language-javascript"><code class="lang-javascript">// Javascript Sample Code
<strong>const { fetch } = require ('cross-fetch')
</strong>const { parseEther } = require("ethers");

const baseUrl = 'https://test.orderbook.premia.finance'

// Free or Paid Tier API KEY
const MOCK_API_KEY = '3423ee2bfd89491f82b351404ea8c3b7'

// Params
const poolAddress='0xb9e594644D3BB24cfcBa3FA0289F18dB7cf81573'
const size= parseEther('5').toString()
const side='bid' //Side is 'ask'('bid') if the intention is to buy(sell). 
const chainId='421613'


async getQuotes(_addr, _size, _side, _chainId){
  const url = `${baseUrl}/quotes?poolAddress=${_addr}&#x26;size=${_size}&#x26;side=${_side}&#x26;chainId=${_chainId}`
	
  const response = await fetch(url, {
    headers: {
      'Content-Type': 'application/json',
      'x-apikey': MOCK_API_KEY
    }
  })
	
  if (!response.ok) {
    console.error('Request failed: ', await response.json())
    throw new Error(`Failed to fetch quote: ${response.statusText}`)
  }
	
  const quotes = await response.json()	
  
  return quotes
}

getQuotes(poolAddress, size, side, chainId)
</code></pre>
{% endtab %}
{% endtabs %}

### <mark style="color:blue;">Get rfq\_quotes</mark>

When publishing an RFQ, users can [listen](websocket.md#subscribe-to-quotes-orderbook-and-rfq) for quotes via WEBSOCKET or use the rfq\_quotes endpoint to retrieve only RFQ quotes via REST API.&#x20;

{% swagger src="../../../.gitbook/assets/openapi3_0.yaml" path="/rfq_quotes" method="get" %}
[openapi3_0.yaml](../../../.gitbook/assets/openapi3_0.yaml)
{% endswagger %}

{% tabs %}
{% tab title="Get RFQ Quote Example " %}
```javascript
// Javascript Sample Code
const { fetch } = require ('cross-fetch')

const baseUrl = 'https://test.orderbook.premia.finance'

// Free or Paid Tier API KEY
const MOCK_API_KEY = '3423ee2bfd89491f82b351404ea8c3b7'

// Params
const poolAddress='0xb9e594644D3BB24cfcBa3FA0289F18dB7cf81573'.toLowerCase()
const side='bid'
const chainId='421613'
const takerAddress = '0xd18EE1c241e7A7e59797763C94d2Bd8C9169c831'


async getRfqQuotes(_addr, _side, _chainId, _taker){
  const url = `${baseUrl}/quotes?poolAddress=${_addr}&side=${_side}&chainId=${_chainId}&taker=${_taker}`
	
  const response = await fetch(url, {
    headers: {
      'Content-Type': 'application/json',
      'x-apikey': MOCK_API_KEY
    }
  })
	
  if (!response.ok) {
    console.error('Request failed: ', await response.json())
    throw new Error(`Failed to fetch quote: ${response.statusText}`)
  }
	
  const rfqQuotes = await response.json()	
  
  return rfqQuotes
}

getRfqQuotes(poolAddress, side, chainId, takerAddress)

```
{% endtab %}
{% endtabs %}

### <mark style="color:blue;">Get Orders</mark>

`GET` orders is a _general purpose_ query of orders in the orderbook.  Many of the params are optional to suit the needs of the query.  Orders are returned in descending order based on the _timestamp_ in which they were created.

{% swagger src="../../../.gitbook/assets/openapi3_0.yaml" path="/orders" method="get" %}
[openapi3_0.yaml](../../../.gitbook/assets/openapi3_0.yaml)
{% endswagger %}

{% hint style="warning" %}
Good To Know: `size` param in `GET` orders is not the same as `size` param in `GET` quotes. See param descriptions.
{% endhint %}

{% tabs %}
{% tab title="Get Orders Example" %}
```javascript
// Javascript Sample Code
const { fetch } = require ('cross-fetch')

const baseUrl = 'https://test.orderbook.premia.finance'

// Free or Paid Tier API KEY
const MOCK_API_KEY = '3423ee2bfd89491f82b351404ea8c3b7'

// Params
const poolAddress='0xb9e594644D3BB24cfcBa3FA0289F18dB7cf81573'
const chainId='421613'


async getOrders(_addr, _chainId){
  const url = `${baseUrl}/orders?poolAddress=${_addr}&chainId=${_chainId}`
	
  const response = await fetch(url, {
    headers: {
      'Content-Type': 'application/json',
      'x-apikey': MOCK_API_KEY
    }
  })
	
  if (!response.ok) {
    console.error('Request failed: ', await response.json())
    throw new Error(`Failed to fetch quote: ${response.statusText}`)
  }
	
  const orders = await response.json()	
  
  return orders
}

getOrders(poolAddress, chainId)
```
{% endtab %}
{% endtabs %}

