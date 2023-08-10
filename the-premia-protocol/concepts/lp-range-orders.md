# LP Range Orders

## <mark style="color:blue;">Overview</mark>

Range orders are at the core of concentrated liquidity, they allow LPs to deposit over a specific price range defined by a _lower price bound_ and _upper price bound_. Orders that span the minimum price width can replicate limit orders similar to orders placed on a limit order-book (if only crossed a single time), while orders spanning across multiple price points (called _ticks_) replicate orders placed _linearly_ over multiple adjacent prices. There is no commitment period for LP deposits. They can be withdrawn prior to expiration.

Premia v3 uses an accounting method called _Split-User Accounting_. At the core, this accounting process allows LPs to define directional exposure of a range order. This is automatically determined based on the asset that is deposited (Collateral or Option Contracts) along with the orientation of the order relative to the current market price. Simply put, range orders provide _uni-directional_ exposure (either buy **or** sell). More information about how _Split-User Accounting_ works can be found in Section 4 of the Premia v3 whitepaper [here](https://premia.finance/v3.pdf).

## <mark style="color:blue;">Range Order (Basics)</mark>

In order to define a range order, 3 primary inputs must be provided to the `deposit` function in the `IPool` interface:

* <mark style="color:orange;">Lower Tick</mark> ⇒ lower price bound
* <mark style="color:orange;">Upper Tick</mark> ⇒ upper price bound
* <mark style="color:orange;">Size (liquidity)</mark> ⇒ size of Collateral OR Option Contracts

{% hint style="info" %}
To find the pool address, users can query the `getPoolAddress` function in `IPoolFactory`.
{% endhint %}

When liquidity is deposited above the market price, this order can be initially viewed as _<mark style="color:red;">ask-side liquidity</mark>_. Similarly, deposits below the market price can be initially viewed as _<mark style="color:green;">bid-side liquidity</mark>_. As the market price moves up and down through a range, the liquidity provider collects maker fees, in addition to any exposure changes.

<figure><img src="../../.gitbook/assets/Screenshot 2023-03-23 at 11.52.39 AM.png" alt=""><figcaption></figcaption></figure>

### Depositing Collateral

If an LP deposits collateral, they are suggesting that they would like to get _long_ option contracts if they are posting a bid-side range order and _short_ contracts if they are posting an ask-side range order. These range orders can be thought of as “buy-to-open” or “sell-to-open” order types initially. As the market price traverses through these range orders, they will become “sell-to-close” and “buy-to-close” range orders respectively.

{% hint style="info" %}
An ask-side range order with collateral can never be net _long_ options just as a bid-side order with collateral can never be _short_ options.
{% endhint %}

### Depositing Option Contracts

If an LP deposits option contracts, they are suggesting that they would like to close their position_._ Bid-side orders can be made with short option contracts, and Ask-side orders can be made with long option contracts. These order types can be thought of as “buy-to-close” and “sell-to-close” order types initially. As markets traverse back and forth through these range orders, an LP may re-establish the original option exposure they deposited with.

{% hint style="info" %}
An ask-side range order with _long options_ can never get net short options. A bid-side order with short options can never get net long options.
{% endhint %}

## <mark style="color:blue;">Range Order (Detailed)</mark>

It is best to illustrate a detailed view of a range order through an example. Here, we will be focusing on a range order with the following details:

* <mark style="color:orange;">Option type</mark> ⇒ Call Option
* <mark style="color:orange;">Current Market Price</mark> ⇒ 0.35
* _<mark style="color:orange;">Initial</mark>_ <mark style="color:orange;"></mark><mark style="color:orange;">Range Order Type</mark> ⇒ _bid-side liquidity_
* <mark style="color:orange;">Lower Tick</mark> ⇒ 0.25
* <mark style="color:orange;">Upper Tick</mark> ⇒ 0.30
* <mark style="color:orange;">Deposit</mark> ⇒ 1 Unit of Base (Underlying) Asset as collateral

As the price of the option moves _down_ (right to left), the collateral which was initially deposited (1 unit) begins to decrease (transferred to takers) as the market price enters and traverses through the order range. The collateral is used to purchase _long_ option contracts from traders.

Once this order is fully traversed (ie. market price is now 0.20), the range order consists of 3.63 options and 0 collateral. Note, any fees collected are not part of the collateral balance and are separately redeemable. At this point, the order is considered sell-side liquidity, which consists of Option Contracts that will be sold to collateral if the price of the option were to go up (left to right).

It is worth noting that pricing is linear, which means the average execution price of a range is simple `(Upper Tick + Lower Tick)/2`. In this example of a sell-side order, the average execution price is 0.275.

<figure><img src="../../.gitbook/assets/Screenshot 2023-03-23 at 12.15.57 PM.png" alt=""><figcaption></figcaption></figure>

### Range Order Width

$$
RangeWidth = upperTick -lowerTick
$$

While the minimum tick precision for a market is 0.001, a range order must have a compatible _RangeWidth_.  Since liquidity for a range order is distributed evenly in between the ticks of a range order, it is possible to generate rounding errors using certain range widths.   While this is automatically handled on the front end to prevent bad order ranges, care must be taken for direct interaction with smart contracts to abide by

A valid _RangeWidth_ for _any_ order type must be one of the following values:

```
ValidRangeWidths = [
  0.001, 0.002, 0.004, 0.005, 0.008, 0.01, 0.016, 0.02, 0.025, 0.032,
  0.04, 0.05, 0.064, 0.08, 0.1, 0.125, 0.128, 0.16, 0.2, 0.25, 0.256, 
  0.32, 0.4, 0.5, 0.512, 0.625, 0.64, 0.8
]
```

#### Example:

A deposit of 1 unit of collateral (ie. WETH) for bid-side liquidity is desired. The lower tick is  .001 and corresponding upper tick is .004 (_RangeWidth_ = 0.003). This would mean that 1/3 of the liquidity would be distributed across each of the 3 ticks within the range.   This produces rounding errors that are undesirable even with 18 decimal places of precision for most ERC-20 assets. &#x20;

## <mark style="color:blue;">Range Order (Technical) - Deposits</mark>

### The Position.Key struct

In order to directly process a range order `deposit`, a position `Key` struct must be passed in. This can be found in the `Position` library. Here, a user can specify both an `owner` and `operator` addresses for the position. This is primarily intended for 3rd party integrations, otherwise both will be the user’s address.

Additionally, a user will specify the `lower` and `upper` ticks for their range order and define the order using the `OrderType` enum as follows:

`uint8` <mark style="color:yellow;">0 → CSUP (Collateral ↔ Short, Use Premiums)</mark> - Convert collateral to short exposure or vice versa and use the premiums towards collateral requirements (fully filled spends any premiums collected).

`uint8` <mark style="color:yellow;">1 → CS (Collateral ↔ Short)</mark> - Convert collateral to short exposure or vice versa and collect (pay) premiums separately (fully filled retains all premiums collected).

`uint8` <mark style="color:yellow;">2 → LC (Long ↔ Collateral)</mark> - Convert collateral to long exposure or vice versa and pay (collect) premiums separately (fully filled spends all collateral as premiums).

### Finding belowLower and belowUpper Ticks

Providing the `belowLower` and `belowUpper` tick values can easily be done by first calling `getNearestTicksBelow` view function with the `lower` and `upper` of the Position to be deposited. The pool will verify that the ticks are in the correct location before inserting and confirming the deposit. The following relationship is verified:

$$
belowLower <= lower <= belowLower.right \\
belowUpper <= upper <= belowUpper.right
$$

{% hint style="info" %}
The insert location is not searched for within each `deposit` transaction, because of the gas costs associated with inserting into the [doubly linked list](https://en.wikipedia.org/wiki/Doubly\_linked\_list) of ticks. By first finding the nearest ticks for the function off-chain (view function) and then verifying correctness on-chain, users can significantly reduce transaction costs per `deposit`.
{% endhint %}

### Determining minimum and maximum market price

In addition to `belowLower`, `belowUpper`, and `size`, users must also supply a `minMarketPrice` and `maxMarketPrice` when directly interacting with the `deposit` function. Market price here refers to the _option’s_ price. By specifying a min/max market price for the option, LPs can get granular control of the composition of asset(s) that are used for collateral. It can be thought of as slippage control for asset composition.

These slippage controls are primarily helpful for orders that are intended to be solely _bid-side_ or _ask-side_ liquidity with a **single** collateral type (option **OR** collateral). When LPs hold a mixture of options and collateral in their wallet, there is risk of using an unintended collateral type if a range order is placed close to the market price.  It is possible for a single sided range order to be an _unintended straddle_ (more on straddles below). This risk can be prevented by correcting defining the `minMarketPrice` and `maxMarketPrice` parameters.

{% hint style="info" %}
If no slippage controls are desired, the values can be set to the minimum and maximum tick for a pool.
{% endhint %}

### Range Orders that straddle market price

It is possible to initialize a range order that _straddles_ the current market price. This would mean `Lower *Tick < Market Price < Upper Tick*`. In this case, a deposit will need to consist of **both** options **AND** collateral (linearly interpolated based on the core order inputs and market price).

Setting the `minMarketPrice` and `maxMarketPrice` to a narrow range ensures the composition of the order is within a user’s tolerance.

### Unintended Straddle Example

Let’s imagine for example an LP holds ETH and short options in their wallet. Their intention is to create a _bid-side_ range order to close the existing short exposure in the price range of 0.4 ↔ 0.5. The range is set just below the current market price of 0.55 (Upper Tick < Market Price). In the process of creating the range order, the market price moves down to 0.45, _into_ the LP’s order range (thus creating an unintended straddle).

In this case, the `deposit` function would seek a small portion of the LP’s ETH and leave behind a small portion of the short option contract in the user’s wallet, to correctly open the position.

Setting the `minMarketPrice` to the Upper Tick (0.5) would cause the transaction to revert if price moved below 0.5, allowing the LP to adjust the range order and re-submit. In this example `maxMarketPrice` is trivial, but still requires a value greater than or equal to the market price at time of deposit.

## <mark style="color:blue;">Range Order (Technical) - Withdraws</mark>

Withdrawing a range order is very similar to a `deposit`. The only difference is that `belowLower` and `belowUpper` are NOT required function inputs, as no linked list insertion is required. The `minMarketPrice` and `maxMarketPrice` parameters are still used to manage slippage and collateral control as mentioned in [Range Order (Technical) - Deposits](lp-range-orders.md#range-order-technical-deposits).

