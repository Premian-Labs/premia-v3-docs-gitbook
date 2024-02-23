---
description: Overview
---

# WebSockets Reference

Their are _two_ major _channels_ within the WS connection, they are:

### [Quotes](quotes-channel.md)

Receive messages regarding _changes_ to the orderbook state along with responses back from an RFQ _requests_&#x20;

[_<mark style="color:green;">Stream</mark>_](./#subscribe-to-quotes-orderbook-and-rfq)  _public_ orderbook state changes such as:

* <mark style="color:orange;">PostQuoteMessage</mark> -> new orders
* <mark style="color:orange;">FillQuoteMessage</mark> -> trade fills
* <mark style="color:orange;">DeleteQuoteMessage</mark> -> cancelled orders

_<mark style="color:green;">Stream</mark>_ _personalized_ quotes from market makers after submitting a RFQ



### [RFQ](rfq-channel.md)

Participate in the RFQ network&#x20;

[_<mark style="color:green;">Publish</mark>_](./#publish-rfq-request-s) RFQ requests to the RFQ network.  This is how requests are broadcasted.

[_<mark style="color:green;">Stream</mark>_](./#subscribe-to-rfq-requests)  RFQ requests in order to _provide_ personalized quotes.  This is how market makers listen for requests.

{% hint style="info" %}
Participating the in the RFQ network (both as a requester and provider of quotes) can _only_ be done via the RFQ channel of our websocket.  There is _no_ direct rest endpoints.
{% endhint %}



### <mark style="color:blue;">Dual Channel Support</mark>

It is possible to integrate BOTH channels at the same time under a single websocket connection.  In this case there only needs to be one authorization methods (see [here](authorization.md) on how to implement this).  However, two filter messages must be sent (one for each channel).&#x20;



Authorizations and Filter messages are covered in the next section.&#x20;
