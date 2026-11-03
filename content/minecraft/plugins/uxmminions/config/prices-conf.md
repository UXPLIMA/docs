---
title: prices.conf
order: 605
description: The seller's own price table.
icon: tag
---

```hocon
prices {
  COBBLESTONE = 0.4
  DIAMOND     = 60.0
  ANCIENT_DEBRIS = 240.0
}
```

What one item is worth before the level's sell multiplier and the tax, used when `file` is reached in `prices.sources`.
A material not here has no price from this source: it stays in the minion. It ships about a hundred lines: stone and
its kinds, ores and ingots, logs, crops, fish, mob drops and meat, wool and more. Changes take a restart.

Related: [Output](../features/output.md#prices)
