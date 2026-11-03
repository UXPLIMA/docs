---
title: Your minions
order: 403
description: Every minion you placed, with the priced teleport, the pocket and the store.
icon: list
---

`/minions` opens `menus/my-minions.conf`: six rows, redrawn every 20 ticks.

| Slot | Item | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34 | Each minion, with its type's icon: level, status, world | Left: its panel. Right: teleport |
| 45, 53 | Arrows | |
| 48 | Ender chest | The [pocket](pocket.md) |
| 49 | Emerald | The [store](store.md); shown only with `uxmminions.store.buy` |

List source `minions:owned`; actions `minions:open` and `minions:teleport`. The pocket button and the pocket file go
together: delete one, delete the other.

Related: [Placing and using](../features/placing-and-using.md)
