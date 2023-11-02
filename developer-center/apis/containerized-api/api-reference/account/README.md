# Account

All endpoints that begin with `/Account/` are primarily related to account balances of collateral, native token (ETH), and options . Setting account collateral approvals can also be done through this entity. &#x20;

{% hint style="success" %}
When trying to fill an order, collateral must be used if you are "opening" a position.  This requires that approvals are set to a minimum of the trade size. If existing option positions exist, approvals are not needed since collateral is not used.&#x20;
{% endhint %}
