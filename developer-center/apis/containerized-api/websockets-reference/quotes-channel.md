# Quotes Channel

The Quote channel is where ALL quotes (limit orders) and state changes to the orderbook can be found. _Additionally, quotes from an RFQ request can also be retrieved here._

### <mark style="color:blue;">Filter Message (Quotes)</mark>

All channels require a filter message prior to streaming.  Filter messages will define what data is streamed along with activating the stream itself. There are several parameters that can be used to refine the stream.

#### <mark style="color:green;">chainId (Required) :</mark> Designate whether the stream is for a production chain or testnet chain

<mark style="color:green;">poolAddress (Optional):</mark> filter by specific option market

<mark style="color:green;">side (Optional):</mark> filter by either bid only or ask only

<mark style="color:green;">size (Optional):</mark> filter by minimum size

{% hint style="warning" %}
The size parameter must be represented in a string that is in 10^18 format.  For example, if the filter for size is mean to be 2, then value here must be '2000000000000000000'.&#x20;
{% endhint %}

<mark style="color:green;">provider (Optional)</mark>: filter by quote providers

<mark style="color:green;">taker (Optional):</mark> _filter by taker address is IMPORTANT for RFQ network users._  There are 3 ways to configure this:

1. <mark style="color:orange;">omit:</mark> if this filter param is omitted, then only public quotes will stream.  If there is no intention of using the RFQ network, omit this filter.
2. <mark style="color:orange;">address:</mark>  if the intention is to use the RFQ network to _request_ quotes, populate this field with the wallet address of the requester to see all relevant `PostQuote` messages from quote providers
3. <mark style="color:orange;">`*`</mark> -> if the intention is to use the RFQ network to _provide/request_ quotes, populate this field with a star to see all relevant `FillQuote` messages.

{% tabs %}
{% tab title="Subscribe/Unsubscribe Quotes" %}
<pre class="language-typescript"><code class="lang-typescript">/*
TypeScript Sample Code
NOTE: since this is an API,  WS logic can be written in any language. This example
is in typescript, but the overall sequence will be the same for any language.
*/

import { RawData, WebSocket } from 'ws'

// Example if Containerized API runtime is local
const url = `ws://localhost:${process.env.HTTP_PORT}`
const wsConnection = new WebSocket(url)
const MOCK_API_KEY = '3423ee2bfd89491f82b351404'


const authMessage: AuthMessage = {
  type: 'AUTH',
  apiKey: MOCK_API_KEY,
  body: null
}

// send AuthMessage
wsConnection.send(JSON.stringify(authMsg))

/*
This filter message will listen to all PUBLIC quote and will additionally
receive PRIVATE quotes from the RFQ network.
*/

const webSocketFilter: FilterMessage = {
  type: 'FILTER',
<strong>  channel: 'QUOTES',
</strong><strong>  body: {
</strong>    chainId: '42161',
    taker: '0x170f9e3eb81ed29491a2efdcfa2edd34fdd24a71' // mock address
  }
}

// handle message ingestion from ws
const wsCallback = (data: RawData) => {
  const message: InfoMessage | PostQuoteMessage | FillQuoteMessage | DeleteQuoteMessage = JSON.parse(data.toString())
    switch (message.type) {
        case 'INFO': {
            // auth result message will arrive here
            break
        }
        case 'POST_QUOTE': {
            // new quotes will arrive here
    	    break
        }
        case 'FILL_QUOTE': {
            // filled quotes will arrive here
    	    break
        }
        case 'DELETE_QUOTE': {
            // cancelled quotes will arrive here
    	    break
        }
        case 'ERROR': {
            // any error message will arrive here
    	    break
        }
        // throw error if case not covered
        default: {
      	    throw `Unexpected message type ${message.type}`
        }
    }
}

// subscribe to WS messages
wsConnection.on('message', wsCallback)

// send FilterMessage to start getting quotes stream
wsConnection.send(JSON.stringify(webSocketFilter))


// unsubscribe to WS messages
// NOTE: unsubscribing does NOT close the ws connection
const unsubscribeMsg: WSUnsubscribeMessage = {
    type: 'UNSUBSCRIBE',
    channel: 'QUOTES',
    body: null,
}

wsConnection.send(JSON.stringify(unsubscribeMsg))
</code></pre>
{% endtab %}

{% tab title="Interface" %}
```typescript
// auth message 
interface AuthMessage {
  type: 'AUTH'
  apiKey: string
  body: null
}

// filter message
interface FilterMessage {
  type: 'FILTER'
  channel: 'QUOTES' | 'RFQ' // which channel to subscribe to
  body: {
    chainId: '42161' | '421613'
    poolAddress?: string
    side?: 'bid' | 'ask'
    size?: string
    provider?: string
    taker?: string
  }
}

// INFO message from Premia orderbook
interface InfoMessage {
  type: 'INFO'
  body: null
  message: string
}

// Quote messages
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

// ERROR message from Premia orderbook
interface ErrorMessage {
  type: 'ERROR'
  body: null
  message: string
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

// Unsubscribe message
interface WSUnsubscribeMessage {
    type: 'UNSUBSCRIBE'
    channel: 'QUOTES' | 'RFQ'
    body: null
}
```
{% endtab %}
{% endtabs %}
