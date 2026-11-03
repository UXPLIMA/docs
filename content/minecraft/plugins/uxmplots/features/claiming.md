---
title: Claiming
order: 402
description: Claiming, auto claim, limits by node, bought slots, giving a plot back, homes and travel.
icon: flag
---

## Claiming

`/plot claim` takes the plot you stand on. `/plot auto` finds the nearest free plot within `plots.auto-search-rings`
(16) rings and takes you there; outside every plot world it searches `plots.auto-world` (or the first world) from its
middle. The wall top turns from `unclaimed-top` to `claimed-top`. A world with a `price` charges first, and
`/plot price` says what the next plot costs.

## Limits

```hocon
limits {
  world-node    = "uxmplots.limit.world."
  total-node    = "uxmplots.limit.total."
  default-total = 0
}
```

A player's limits come from nodes: `uxmplots.limit.world.<n>` per world and `uxmplots.limit.total.<n>` across all,
the highest number held winning, or `unlimited`. Without a node the world's `max-plots-per-player` and
`default-total` apply; 0 is no limit. A merged area counts as many plots as it covers. [Levels](levels.md) add extra
plots.

## Buying room

```hocon
slots {
  most     = 0
  currency = ""
  first    = 500
  step     = 500
  curve    = "linear"
}
```

`/plot slots buy` sells room for one more plot, added to the node's number and never instead of it, and never taken
back. Each costs more than the last (`flat`, `linear` or `geometric`). Ships off: `most = 0` and no currency.

## Giving it back

`/plot abandon` asks twice. A world with a `refund-share` pays part of the price back.

## Travel

`/plot home [number]` goes to one of yours, `/plot visit <player or plot>`, `/plot tp <plot>` and `/plot random` go
elsewhere, when the visit policy allows. `/plot outline` draws the edge in the air (`personality.border`).

Related: [Plot worlds](worlds.md#price), [Members and visitors](members.md)
