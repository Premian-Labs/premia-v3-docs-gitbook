# Pool

All endpoints that begin with `/Pool/` are related to actions surrounding _position_ (inactive) & _collateral_ release.  In order to make sure that collateral is not unnecessarily tied up, these end points will need to be called from time to time to help _<mark style="color:orange;">release</mark>_ your collateral in positions that do not represent _<mark style="color:orange;">active</mark>_ exposure. &#x20;

{% hint style="success" %}
The [Options Balances](../account/option-balances.md) endpoint can be used to determine what _inactive_ positions are held.&#x20;
{% endhint %}
