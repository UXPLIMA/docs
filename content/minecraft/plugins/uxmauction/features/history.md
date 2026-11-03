---
title: History
order: 409
description: What an item has sold for, and how long sales are kept.
icon: clock
---

Every finished sale writes one row of its own, and that row outlives the listing it came from. `/auction history` opens
the [history window](../menus/search.md#history) for the item in your hand: what it has sold for, newest first.

| Key under `history` | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Keep sales |
| `keep-for` | `"30d"` | How old a row may get before the sweep deletes it; `"0"` keeps nothing |

An old price is not the price, and a house that never forgets keeps one row per sale for ever.

`%uxmauction_top_price_<MATERIAL>%` reads what the most anybody asks for one material now, from the shelf.

Related: [History window](../menus/search.md#history)
