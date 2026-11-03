---
title: Currencies
order: 416
description: The five currency types, the shipped diamonds, and how prices, fees, rents and tips pick one.
icon: coins
---

`currencies.conf`. Every price, fee, refund, rent and tip names a currency id; nothing is ever converted.

| Id | Type | Order | Ships |
|---|---|---|---|
| `diamonds` | `item`, `material = "DIAMOND"` | 10 | on |
| `coins` | `vault` | 20 | off |
| `experience` | `experience`, in points | 30 | off |
| `custom` | `placeholder`: `balance`, `give`, `take` | 40 | off |
| `treasury` | `treasury`: `treasury-currency`, `wait-millis` 2000 | 50 | off |

The lowest `order` is the one a price with no name is in. Turn a currency off with `enabled = false` rather than
deleting it. A currency whose plugin is missing answers that it is not ready and nothing is priced in it.

An item, experience and Treasury live inside a player, so they can only be reached while that player is online; a
builder tipped in them while away is told rather than paid. A placeholder currency fills `{player}` with the uuid and
`{amount}` with the number.

Related: [currencies.conf](../config/currencies-conf.md)
