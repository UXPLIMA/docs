---
title: Upgrade
order: 407
description: The next level, each requirement in your own words, buying, and consuming.
icon: arrow-up-circle
---

`menus/upgrade.conf`: five rows, redrawn every 20 ticks.

| Slot | Item | Click |
|---|---|---|
| 11 | Book: this level's interval, range and storage | |
| 15 | Enchanted book: the next level's | |
| 28 to 34 | Each requirement, with its own message | |
| 36 | Arrow, back | The panel |
| 40 | Experience bottle | Buys the next level |
| 44 | Minecart | Feeds your least worked minion of this type and level to this one |

List source `minions:requirements`; actions `minions:buy-level` and `minions:consume`. The minecart says it is off
until `upgrades.consume-minion` is on.

Related: [Upgrades](../features/upgrades.md)
