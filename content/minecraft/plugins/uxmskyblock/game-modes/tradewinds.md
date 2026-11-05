---
title: TradeWinds
order: 312
description: A trading vessel on a sea of its own, between ports.
icon: ship
---

A TradeWinds island is a trading vessel on a sea of its own. The team sails it between the ports the file names, buys
low and sells high from the vessel's cargo hold, and climbs the ranks.

## Sailing and trading

| Command | What it does |
|---|---|
| `/is sail` | Chooses the port the vessel sails for; a voyage takes time |
| `/is market` | Buys into the hold and sells from it, paid by the island bank |
| `/is contracts` | Takes a contract to deliver goods to a port in time, and delivers it |
| `/is cargo` | Opens the cargo hold of the vessel you are aboard |
| `/is plunder` | Takes part of another vessel's hold, at sea, when piracy is on |

Prices move with what is wanted that day, and each port buys and sells its own goods. At sea a storm can hold the
vessel back, pirates can ask a toll from the bank, and the crew can haul a find out of the sea. Customs at a port can
seize contraband from the hold.

## Ranks

A vessel climbs the ranks, lowest first, by the trade it has done: Dinghy, Sloop, Brigantine and Galleon as shipped.
Each rank gives the hold more rows. Ports (Emerald Bay, Saltmarsh, Ironhaven as shipped), goods, prices, contracts,
hazards, customs and piracy are in [tradewinds.conf](../config/tradewinds-conf.md). Staff verbs:
`/isadmin tradewinds setrank`, `setstanding` and `moveport`.
