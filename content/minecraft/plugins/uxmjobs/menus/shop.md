---
title: Shop
order: 410
description: The job shop, where points are spent.
icon: store
---

`/jobs shop` opens `menus/shop.conf`: five rows. A line the viewer has not reached is not drawn.

| Slot | Item | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34 | Each line, drawn with its `icon`: cost and the balance | Buys it |
| 40 | Sunflower: the balance | |
| 44 | Barrier, close | |

Tokens: `%shop_name%`, `%shop_description%`, `%shop_icon%`, `%shop_cost%`, `%shop_affordable%` (`true` when the viewer
holds enough), `%player_points%`. List source `jobs:shop`; action `jobs:buy`.

Related: [Points and the shop](../features/points-and-shop.md)
