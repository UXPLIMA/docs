---
title: Admin
order: 412
description: Another player's minions, by id, for an operator.
icon: shield
---

`/uxmminions minions <player>` (`uxmminions.admin.minions`) opens `menus/admin.conf`: six rows, redrawn every 20 ticks.

| Slot | Item | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34 | Each minion: id, level, world | Left: its panel. Shift right: removes it, written to the audit |
| 45, 53 | Arrows | |

List source `minions:of-player`; actions `minions:open` and `minions:admin-remove`.

Related: [Admin commands](../commands/admin.md)
