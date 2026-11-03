---
title: Market and warps
order: 307
description: Selling claims, public warps with fees, visits and favourites.
icon: store
---

## Selling

`/claim sell <price>` lists a claim and `/claim buy` buys the one you stand in; `/claim market` lists everything for
sale or rent. A buyer gets the claim empty: no members, bans or tenants. A seller who is away is paid at their next
join. A claim for sale says its price on the web maps.

## Warps

`/claim setspawn` sets where teleports land and `/claim warp open [fee]` lets every player teleport there with
`/claim tp <claim>`, paying the owner the fee (up to `economy.max-visit-fee`). `/claim warp close` closes it. Visits
count towards the [visits board](boards.md). The claims window lists public warps on its own tab.

## Favourites

`/claim favourite` keeps the claim you stand in on your favourites tab, to reach it again from the
[claims window](../menus/claims.md).

Related: [Market commands](../commands/areas-and-market.md#market)
