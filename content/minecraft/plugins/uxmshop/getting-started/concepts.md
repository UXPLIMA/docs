---
title: Concepts
order: 103
description: Sections, lines, directions, the till, offers and currencies.
icon: lightbulb
---

| Word | Means |
|---|---|
| Catalogue | Every section under `shops/`; the server is the counterparty |
| Section | One file; its file name is its id, the argument of `/shop open`; open to everybody unless its `permission` names a node |
| Line | One product of a section |
| Direction | `buy` (the player pays) or `sell` (the player is paid); leaving a price out turns that direction off |
| Till | The one path every trade takes, whichever command or window started it, so every rule holds everywhere |
| Player shop | A container in the world with lines (offers) and stock kept in the database |
| Offer | One line of a player shop, numbered for `/myshop` commands |
| Currency | An id from `currencies.conf`; nothing is ever converted |

On a purchase money moves before the item, and on a sale the item before the money, so a trade that fails at any step
leaves nothing moved. The log row, the counters and the price are written together.

Related: [Sections and lines](../features/sections-and-lines.md), [Player shops](../features/player-shops.md)
