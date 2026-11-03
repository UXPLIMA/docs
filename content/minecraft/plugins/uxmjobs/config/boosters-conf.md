---
title: boosters.conf
order: 607
description: How boosters combine, and permission boosters.
icon: rocket
---

| Key | Meaning |
|---|---|
| `combine` | `highest` (only the largest counts) or `summed` (the extra parts add) |
| `permission.<name>.node` | The node you wrote |
| `permission.<name>.multiplier` | An expression |
| `permission.<name>.jobs` | Jobs it covers; empty covers all |
| `permission.<name>.currencies` | `money`, `experience`, `points`; empty covers all |

Nothing ships under `permission`. See [Boosters](../features/boosters.md).

Related: [schedule.conf](schedule-conf.md)
