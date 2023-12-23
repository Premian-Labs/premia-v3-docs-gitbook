---
description: Websocket Functionality
---

# WebSockets Reference

### <mark style="color:blue;">Overview</mark>

Using the WS connection, it is possible to:

* [_<mark style="color:blue;">Stream</mark>_](websockets-reference.md#subscribe-to-quotes-orderbook-and-rfq) public/RFQ quotes from the orderbook in realtime
* [_<mark style="color:green;">Publish</mark>_](websockets-reference.md#publish-rfq-request-s) an RFQ requests (for market takers) to _receive personalized_ quotes
* [_<mark style="color:green;">Stream</mark>_](websockets-reference.md#subscribe-to-rfq-requests) _RFQ_ requests (for market makers) to subsequently provide personalize quotes

### <mark style="color:blue;">Authorization Flow</mark>

Upon connection with the Websocket server, users must send an `AUTH` message with an api key.  If this step is not done, any requests to _Stream_ quotes or _Publish_ RFQ requests will be denied.&#x20;

{% tabs %}
{% tab title="Authorization Websocket Example" %}
```typescript
// TypeScript Sample Code 
import { RawData, WebSocket } from 'ws'

// Example for for local container runtime
const url = `ws://localhost:${process.env.HTTP_PORT}`
const wsConnection = new WebSocket(url)
const MOCK_API_KEY = '3423ee2bfd89491f82b351404'

// typings
interface AuthMessage {
  type: 'AUTH'
  apiKey: string
  body: null
}

const authMessage: AuthMessage = {
  type: "AUTH",
  apiKey: MOCK_API_KEY,
  body: null
}

const wsCallback = (data: RawData) => {
  const message: InfoMessage | ErrorMessage | RFQMessage = JSON.parse(data.toString())
  // implement your business logic
}

// subscribe to WS messages
wsConnection.on('message', wsCallback)

// send AuthMessage
wsConnection.send(JSON.stringify(authMsg))

// unsubscribe if you want to modify callback function and subscribe again
wsConnection.off('message', wsCallback)
```


{% endtab %}
{% endtabs %}

### <mark style="color:blue;">Subscribe to Quotes (Orderbook & RFQ)</mark>

Subscribing to quotes allows a user to stream real time quotes that are published to the orderbook along with any private quotes that may also be available from an [RFQ request](websockets-reference.md#publish-rfq-request-s).  By default, ALL public quotes are subscribed to upon authorization of a websocket connection. `FILTER` message are required to start listening to orderbook _quotes events_: a quote published, filled or cancelled.

{% hint style="info" %}
Filtering by `provider` is _exclusive_: WS subscription will listen ONLY to the quotes where `quote.provider == provider`. In contrast, `taker` filter is _additive:_ WS subscription will receive all public quotes (`quote.taker == ZeroAddress`) _and_ private quotes (`quote.taker` is valid wallet address, e.g. not `ZeroAddress`).
{% endhint %}

{% tabs %}
{% tab title="Subscribe to Quotes Stream Exmaple" %}
<pre class="language-typescript"><code class="lang-typescript">// TypeScript Sample Code 
import { RawData, WebSocket } from 'ws'

// Example for for local container runtime
const url = `ws://localhost:${process.env.HTTP_PORT}`
const wsConnection = new WebSocket(url)
const MOCK_API_KEY = '3423ee2bfd89491f82b351404'

interface AuthMessage {
  type: 'AUTH'
  apiKey: string
  body: null
}

const authMessage: AuthMessage = {
  type: 'AUTH',
  apiKey: MOCK_API_KEY,
  body: null
}

// send AuthMessage
wsConnection.send(JSON.stringify(authMsg))

/*
Optional params:
<strong>  poolAddress -> specific option market
</strong>  size -> stringified 10^18 value
  side -> 'bid' or 'ask'
  taker -> taker address (use if publishing RFQ and want to receive personal quotes)
  provider -> quote provider address
 */

interface FilterMessage {
  type: 'FILTER'
  channel: 'QUOTES' | 'RFQ'
  body: {
    poolAddress?: string
    side?: 'bid' | 'ask'
    // ARBITRUM or ARBITRUM GOERLI
    chainId: '42161' | '421613'
    // bigInt string representation
    size?: string
    taker?: string
    provider?: string
  }
}

// subscribe to listen to quotes
const webSocketFilter: FilterMessage = {
  type: 'FILTER',
<strong>  channel: 'QUOTES',
</strong><strong>  body: {
</strong>    chainId: '42161',
    taker: '0x170f9e3eb81ed29491a2efdcfa2edd34fdd24a71' // fake address
  }
}

// Quote events messages types
interface PostQuoteMessage {
  type: 'POST_QUOTE'
  body: ReturnedOrderbookQuote
}

interface FillQuoteMessage {
  type: 'FILL_QUOTE'
  body: ReturnedOrderbookQuote
  size: string
}

interface DeleteQuoteMessage {
  type: 'DELETE_QUOTE'
  body: ReturnedOrderbookQuote
}

// Quote object
interface ReturnedOrderbookQuote {
  base: string // token name
  quote: string // token name
  expiration: string
  strike: number
  type: 'C' | 'P'
  side: 'bid' | 'ask'
  size: number
  price: number
  deadline: number
  quoteId: string
  ts: number
}

const wsCallback = (data: RawData) => {
  const message: InfoMessage | PostQuoteMessage | FillQuoteMessage | DeleteQuoteMessage = JSON.parse(data.toString())
  // implement your business logic
}

// subscribe to WS messages
wsConnection.on('message', wsCallback)

// send FilterMessage to start getting quotes stream
wsConnection.send(JSON.stringify(webSocketFilter))
</code></pre>
{% endtab %}
{% endtabs %}

### <mark style="color:blue;">Publish & Subscribe RFQ Request(s)</mark>

If the desire is to send an RFQ request to receive personalized quotes, it must be done by sending an `RFQ` message type.  Please note that in order to RECEIVE these personalized quotes via websocket, the `FILTER` message when subscribing to quotes must include the `taker` address.&#x20;

As a _market maker_ who would like to provide quotes to users who want to utilize RFQ, it will require subscribing to requests to be alerted when a request is made.  In order to respond to an RFQ, market maker must publish quotes (via REST api), with the `takerAddress` populated with the requesters address, otherwise the quote is fillable by any party and will likely be missed by the requester who is listening for quotes with their address populated in the `takerAddress` field of a quote.

The following example a has complete end-to-end flow from authorization to publishing and listening to RFQ messages.

{% tabs %}
{% tab title="Publish RFQ Example" %}
```javascript
// TypeScript Sample Code 
import { RawData, WebSocket } from 'ws'

// Example for for local container runtime
const url = `ws://localhost:${process.env.HTTP_PORT}`
const wsConnection = new WebSocket(url)
const MOCK_API_KEY = '3423ee2bfd89491f82b351404'

interface AuthMessage {
  type: 'AUTH'
  apiKey: string
  body: null
}

const authMessage: AuthMessage = {
  type: 'AUTH',
  apiKey: MOCK_API_KEY,
  body: null
}

// send AuthMessage
wsConnection.send(JSON.stringify(authMsg))

// INFO message from Premia orderbook
interface InfoMessage {
  type: 'INFO'
  body: null
  message: string
}

// ERROR message from Premia orderbook
interface ErrorMessage {
  type: 'ERROR'
  body: null
  message: string
}

// human-readable format
interface RFQMessage {
  type: 'RFQ'
  body: {
    base: string // base token name i.e. WETH
    quote: string // base token name i.e. USDC
    expiration: string // i.e. 23NOV2023
    strike: number
    type: 'C' | 'P'
    side: 'bid' | 'ask'
    size: number
    taker: string
  }
}

// IMPORTANT!!! outbound RFQ message is web3 format
// We recommended replicate/copy createPoolKey function
const rfqRequest = {
  type: 'RFQ',
  body: {
    poolKey: {
      	base: '0x7F5bc2250ea57d8ca932898297b1FF9aE1a04999'
	quote: '0x53421DB1f41368E028A4239954feB5033C7B3729'
	oracleAdapter: string
	// serialised bigint
	strike: parseEther('1700').toString()
	// make sure maturities either tomorrow, 
	// the day after tomorrow, 
	// each Friday of current month, 
	// last Friday of each next month
	// 8:00 AM UTC
	maturity: 1702022400
	isCallPool: boolean
    },
    side: 'ask',
    chainId: '421613', // or '42161' for prod environment
    size: parseEther('1').toString(),
    taker: '0x3D1dcc44D65C08b39029cA8673D705D7e5c4cFF2'
  }
}

const wsCallback = (data: RawData) => {
  const message: InfoMessage | ErrorMessage | RFQMessage | PostQuoteMessage | FillQuoteMessage | DeleteQuoteMessage = JSON.parse(data.toString())
    switch (message.type) {
      case 'RFQ': {
	// receives rfq stream
	break
      }
      case 'INFO': {
        // auth result message will arrive here
        break
      }
      case 'ERROR': {
        // any error message will arrive here
        break
      }
      default: {
        throw `Unexpected message type ${message.type}`
      }
}

// subscribe to WS messages
wsConnection.on('message', wsCallback)

// send RFQMessage to start getting quotes stream
wsConnection.send(JSON.stringify(rfqRequest))

const webSocketFilter: FilterMessage = {
  type: 'FILTER',
  channel: 'RFQ',
  body: {
    chainId: '42161',
    taker: '0x170f9e3eb81ed29491a2efdcfa2edd34fdd24a71' // fake address
  }
}

// send FilterMessage to start listening to RFQ stream
wsConnection.send(JSON.stringify(webSocketFilter))
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
IMPORTANT:  WS API supports both `FilterMessage` channels `RFQ` and `QUOTES` running concurrently. However, only single filter can be applied  per channel type. This means if you're listening to both quotes and RFQ streams sending new message filter into`QUOTES`channel will override quotes streams filter, but RFQ will still run without changes, e.g. there's only one singleton filter state per channel.
{% endhint %}

### <mark style="color:blue;">Unsubscribing from WS Streams</mark>

User can stop listening to either `RFQ` or `QUOTES` streams channels or _both_ without closing WS session using UNSUBSCRIBE command as following:

```typescript
interface WSUnsubscribeMessage {
    type: 'UNSUBSCRIBE'
    channel: 'QUOTES' | 'RFQ'
    body: null
}

const unsubscribeMsg: WSUnsubscribeMessage = {
    type: 'UNSUBSCRIBE',
    channel: channel,
    body: null,
}

wsConnection.send(JSON.stringify(unsubscribeMsg))
```





<mark style="color:blue;">TODO</mark>
