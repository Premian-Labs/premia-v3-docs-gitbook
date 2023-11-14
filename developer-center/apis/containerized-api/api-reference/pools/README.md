# Pools

Endpoints that begin with `/pools/` path provides user control over option pools lifecycle: use can fetch all available pools (optionally filtered by `base` token, `quote` token, `expiration` date) and deploy a new option pool.

{% hint style="success" %}
If some option pools is _already deployed,_ associated APIs will handle this case gracefully without an error.
{% endhint %}
