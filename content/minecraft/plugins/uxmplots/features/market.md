---
title: Market
order: 311
description: Selling a plot, renting one out, and taking one for a while.
icon: badge-dollar-sign
---

## Selling

`/plot sell <price> [currency]` puts the plot you stand on up for sale; `/plot sell cancel` takes it off; `/plot market`
lists what is for sale. A buyer stands on it and types `/plot buy`. The buyer pays, then the seller is paid, and only
then does the plot move: the build, the members, the bans, the settings and everything anybody said about it. A seller
whose currency cannot reach them offline stops the sale before the buyer is charged.

## Renting

On your own plot, `/plot rent <price> <currency> <days>` puts it up; on somebody else's, `/plot rent` takes it for that
period. The owner is paid before the tenant may build. A period that runs out takes away the right to build and nothing
else: the blocks, the plot and the row stay. `/plot rent cancel` takes the sign down and anybody renting keeps their
time. `/plot renting` lists what you rent and rent out.

Related: [Currencies](currencies.md)
