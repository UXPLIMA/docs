---
title: Boosters
order: 507
description: What is boosting the viewer, and of which kind.
icon: zap
---

`/jobs boosts` opens `menus/boosters.conf`: three rows, a nether star per booster that applies.

| Slot | Item |
|---|---|
| 10 to 16 | A booster: its multiplier and kind |
| 21, 23 | Arrows |
| 22 | Barrier, close |

`%booster_kind%` is `permission`, `timed` or `scheduled`, and picks the block `menu.boosters.kind.<kind>`;
`%booster_multiplier%` is the number. List source `jobs:boosters`.

Related: [Boosters](../features/boosters.md)
