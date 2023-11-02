---
description: Websocket Functionality
---

# WEBSOCKET

### <mark style="color:blue;">Overview</mark>

| Chain                    | Websocket Url                    |
| ------------------------ | -------------------------------- |
| Arbitrum Goerli (421613) | wss://test.quotes.premia.finance |
| Arbitrum One (42161)     | wss://quotes.premia.finance      |

Using the WEBSOCKET connection, it is possible to:

* [_<mark style="color:blue;">Stream</mark>_](websocket.md#subscribe-to-quotes-orderbook-and-rfq) public/RFQ quotes from the orderbook in realtime
* [_<mark style="color:green;">Publish</mark>_](websocket.md#publish-rfq-request-s) an RFQ requests (for market takers) to _receive personalized_ quotes
* [_<mark style="color:green;">Stream</mark>_](websocket.md#subscribe-to-rfq-requests) _RFQ_ requests (for market makers) to subsequently provide personalize quotes

### <mark style="color:blue;">Authorize Websocket</mark>

Upon connection with the Websocket server, users must send an `AUTH` message with an api key.  If this step is not done, any requests to _Stream_ quotes or _Publish_ RFQ requests will be denied.&#x20;



{% tabs %}
{% tab title="Authorization Websocket Example" %}
```javascript
// Javascript Sample Code
const { WebSocket } = require('ws')

const wsUrl = 'test.quotes.premia.finance'
const MOCK_API_KEY = '3423ee2bfd89491f82b351404ea8c3b7'

const authMessage = {
  type: "AUTH",
  apiKey: MOCK_API_KEY,
  body: null
}


const ws = new WebSocket(`wss://${wsUrl}`)
function connectWebsocket() {
  ws.onopen = (event) => {
    console.log("Websocket connection open")
    ws.send(JSON.stringify(authMessage))
    ws.onmessage = (event) => {
      console.log(JSON.parse(event.data).message)
      ws.close()
    }
  }
}

connectWebsocket()
```


{% endtab %}
{% endtabs %}

### <mark style="color:blue;">Subscribe to Quotes (Orderbook & RFQ)</mark>

Subscribing to quotes allows a user to stream real time quotes that are published to the orderbook along with any private quotes that may also be available from an [RFQ request](websocket.md#publish-rfq-request-s).  By default, ALL public quotes are subscribed to upon authorization of a websocket connection. `FILTER` messages are required.



{% tabs %}
{% tab title="Subscribe to Filtered Quotes Example" %}
```javascript
// Javascript Sample Code
const { WebSocket } = require('ws')
const { parseEther } = require("ethers");

const wsUrl = 'test.quotes.premia.finance'
const MOCK_API_KEY = '3423ee2bfd89491f82b351404ea8c3b7'

const authMessage = {
  type: "AUTH",
  apiKey: MOCK_API_KEY,
  body: null
}

/*
Optional params in body include:
poolAddress -> specific option market
size -> stringified 10^18 value
side -> 'bid' or 'ask'
taker -> taker address (use if publishing RFQ and want to receive personal quotes)
provider -> quote provider address
 */
const filterMessage = {
  type: 'FILTER',
  channel: 'QUOTES',
  body: {
    chainId: '421613',
    poolAddress: '0xeB785e131784ea28b6B64B605CF8aa83D69793b5'
  }
}

const ws = new WebSocket(`wss://${wsUrl}`)
function connectWebsocket() {
  ws.onopen = (event) => {
    console.log("Websocket connection open")
    ws.send(JSON.stringify(authMessage))
    ws.onmessage = (event) => {
      console.log(JSON.parse(event.data).message)
      sendMessage(filterMessage)
    }
  }
}

function sendMessage(_message){
  ws.onmessage = (event) => {
    console.log(JSON.parse(event.data).message)
  }
  ws.send(JSON.stringify(_message))
  setTimeout(() => {
    console.log("Closing connection")
    ws.close()
  }, 1000)
}

connectWebsocket()
		
```
{% endtab %}
{% endtabs %}

### <mark style="color:blue;">Publish RFQ Request(s)</mark>

If the desire is to send an RFQ request to receive personalized quotes, it must be done by sending an `RFQ` message type.  Please note that in order to RECEIVE these personalized quotes via websocket, the `FILTER` message when subscribing to quotes must include the `taker` address.  Alternatively, it is possible to use the get [rfqQuotes](rest-api.md#get-rfq\_quotes) REST API endpoint.

{% tabs %}
{% tab title="Publish RFQ Example" %}
```javascript
// Javascript Sample Code
const { WebSocket } = require('ws')
const { parseEther } = require("ethers");

const wsUrl = 'test.quotes.premia.finance'
const MOCK_API_KEY = '3423ee2bfd89491f82b351404ea8c3b7'

const authMessage = {
  type: "AUTH",
  apiKey: MOCK_API_KEY,
  body: null
}

// All fields are required
const rfqPublishMessage = {
  type: 'RFQ',
  body: {
    poolAddress: '0xeB785e131784ea28b6B64B605CF8aa83D69793b5',
    side: 'ask',
    chainId: '421613',
    size: parseEther('1').toString(),
    taker: '0x3D1dcc44D65C08b39029cA8673D705D7e5c4cFF2'
  }
}

const ws = new WebSocket(`wss://${wsUrl}`)
function connectWebsocket() {
  ws.onopen = (event) => {
    console.log("Websocket connection open")
    ws.send(JSON.stringify(authMessage))
    ws.onmessage = (event) => {
      console.log(JSON.parse(event.data).message)
      sendMessage(rfqPublishMessage)
    }
  }
}

function sendMessage(_message){
  ws.onmessage = (event) => {
    console.log(JSON.parse(event.data).message)
  }
  ws.send(JSON.stringify(_message))
  setTimeout(() => {
    console.log("Closing connection")
    ws.close()
  }, 1000)
}

connectWebsocket()
```
{% endtab %}
{% endtabs %}

### <mark style="color:blue;">Subscribe to RFQ Requests</mark>

As a market maker who would like to provide quotes to users who want to utilize RFQ, it will require subscribing to requests to be alerted when a request is made.  In order to respond to an RFQ, market maker must publish quotes (via rest api), with the `takerAddress` populated with the requesters address, otherwise the quote is fillable by any party and will likely be missed by the requester who is listening for quotes with their address populated in the `takerAddress` field of a quote.

{% tabs %}
{% tab title="Subscribe to RFQ Example" %}
```javascript
// Javascript Sample Code
const { WebSocket } = require('ws')

const wsUrl = 'test.quotes.premia.finance'
const MOCK_API_KEY = '3423ee2bfd89491f82b351404ea8c3b7'

const authMessage = {
  type: "AUTH",
  apiKey: MOCK_API_KEY,
  body: null
}

/*
chainId is required.
Optional params in body include:
poolAddress -> specific option market
side -> 'bid' or 'ask'
 */
const rfqListenMessage= {
  type: 'FILTER',
  channel: 'RFQ',
  body: {
    chainId: '421613',
  },
}

const ws = new WebSocket(`wss://${wsUrl}`)
function connectWebsocket() {
  ws.onopen = (event) => {
    console.log("Websocket connection open")
    ws.send(JSON.stringify(authMessage))
    ws.onmessage = (event) => {
      console.log(JSON.parse(event.data).message)
      sendMessage(rfqListenMessage)
    }
  }
}

function sendMessage(_message){
  ws.onmessage = (event) => {
    console.log(JSON.parse(event.data).message)
  }
  ws.send(JSON.stringify(_message))
  setTimeout(() => {
    console.log("Closing connection")
    ws.close()
  }, 1000)
}

connectWebsocket()
```
{% endtab %}
{% endtabs %}
