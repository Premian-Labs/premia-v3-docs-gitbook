# Connection

All WS connections have a 30 minute connection duration. Users must incorporate reconnection logic in order to maintain streaming uptime of channels.  An example of this can be found here:

{% tabs %}
{% tab title="Reconnect Example" %}
```typescript
/*
TypeScript Sample Code
NOTE: since this is an API,  WS logic can be written in any language. This example
is in typescript, but the overall sequence will be the same for any language.
*/

import { WebSocket } from 'ws'

// Example if Containerized API runtime is local
const url = `ws://localhost:${process.env.HTTP_PORT}`
const MOCK_API_KEY = '3423ee2bfd89491f82b351404'

const authMessage: AuthMessage = {
  type: "AUTH",
  apiKey: MOCK_API_KEY,
  body: null
}

const connect = () => {
  const wsConnection = new WebSocket(url)
  
  wsConnection.on('open', function() {
    // send AuthMessage to authorize connection 
    wsConnection.send(JSON.stringify(authMsg))
  });
  
  wsConnection.on('close', function() {
   // reconnect after 500 ms
    setTimeout(connect, 500);
  });
  
}

connect()
```
{% endtab %}
{% endtabs %}
