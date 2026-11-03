---
title: Prices
order: 303
description: Dynamic bands, ranks, modifiers, the order they apply in, rounding and tax.
icon: badge-dollar-sign
---

A number is a valid price on its own. Beyond that:

## Dynamic band

```hocon
band {
  enabled                = true
  move-per-trade         = 0.001
  ceiling                = 2.0
  floor                  = 0.5
  normalisation          = 0.05
  normalisation-interval = "30m"
}
```

| Key | Default | Meaning |
|---|---|---|
| `move-per-trade` | 0.001 | How much of the base price one item moves the price |
| `ceiling`, `floor` | 2.0, 0.5 | The limits, as multiples of the base |
| `normalisation` | 0.05 | How much of the distance home is walked each interval |
| `normalisation-interval` | `30m` | How often |

On a line or a section; off unless `enabled = true`. There is no expression language. `/uxmshop resetprice` puts a
line back on its base.

## Ranks

```hocon
shop {
  ranks {
    vip { permission = "uxmshop.rank.vip", buy = 0.95, sell = 1.05 }
  }
}
```

`buy` and `sell` multiply the price for a player who holds the rank's node (`uxmshop.rank.<rank>` when none is named).
A player with two ranks takes the better one in each direction; they never stack. None ship.

## Modifiers

`/uxmshop modifier` gives one player a multiplier over everything, a section or a line. The narrowest that names the
trade wins; they never stack. `modifier clear` removes them.

## The order

Base price, band, best rank, narrowest modifier, the line's `floor` and `ceiling`, the quantity, then tax.

## Rounding

A buy rounds up and a sell rounds down, to the places the currency holds. A shown price with more places than the
currency's `decimals` is rounded up, so a price shown is never under the price charged.

## Tax

`shop.tax.percent` (0) is taken out of what a player is paid for a sale and never added to what they pay, so the price
on a tile is what is charged. A holder of `shop.tax.bypass` (`uxmshop.bypass.tax`) pays none. Player shops have their
own `player-shops.tax.percent`.

Related: [config.conf](../config/config-conf.md#shop)
