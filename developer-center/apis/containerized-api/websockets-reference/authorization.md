# Authorization

### <mark style="color:blue;">Authorization Flow</mark>

Upon connection with the Websocket server, users must first send an `AUTH` message with an api key.  If this step is not done, any requests to _<mark style="color:blue;">Stream</mark>_ public/RFQ quotes, _<mark style="color:green;">Publish</mark>_ RFQ requests, or _<mark style="color:blue;">Stream</mark>_ RFQ requests will be denied.&#x20;



### <mark style="color:blue;">API KEY</mark>

In order to use websockets, an api key is required.  Currently, all api key requests must be made to <mark style="color:blue;">research@premia.finance</mark>.  Please use subject line: 'API KEY REQUEST' and an api key will be returned. &#x20;



{% tabs %}
{% tab title="Authorization " %}
```typescript
/*
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
  type: "AUTH",
  apiKey: MOCK_API_KEY,
  body: null
}

// initialize variable to store authorization response
let infoMessage = ''

// handle message ingestion from ws
const wsCallback = (data: RawData) => {
  const message: InfoMessage | ErrorMessage | RFQMessage = JSON.parse(data.toString())
  
  switch (message.type) {
    case 'INFO': {
        // auth result message will arrive here
	infoMessage = message.message
	break
    }
    default: {
  	throw `Unexpected message type ${message.type}`
    }
  }
}

// subscribe/listen to WS messages
wsConnection.on('message', wsCallback)

// send AuthMessage to authorize connection 
wsConnection.send(JSON.stringify(authMsg))

// unsubscribe/remove message listener
wsConnection.off('message', wsCallback)

// should say `Session authorized. Subscriptions enabled.`
console.log(infoMessage)
```


{% endtab %}

{% tab title="Interface" %}
```typescript
// auth message 
interface AuthMessage {
  type: 'AUTH'
  apiKey: string
  body: null
}

// INFO message from Premia orderbook
interface InfoMessage {
  type: 'INFO'
  body: null
  message: string
}
```
{% endtab %}
{% endtabs %}

