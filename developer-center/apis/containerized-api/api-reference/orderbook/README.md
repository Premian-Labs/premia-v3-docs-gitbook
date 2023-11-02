# Orderbook

All endpoints that begin with `/orderbook/` are related to actions surrounding _<mark style="color:orange;">quotes</mark>_ and _<mark style="color:orange;">trading</mark>_ on the orderbook. The core trading logic and the required data needed to trade can be found within this entity.

{% hint style="info" %}
While receiving quotes does not require any additional coding logic other than a rest api call, please be aware that when trading, it is important to make sure that you have approved the movement of your collateral price to filling an order.  More details on this can be found in [Collateral Approval](../account/collateral-approval.md).
{% endhint %}
