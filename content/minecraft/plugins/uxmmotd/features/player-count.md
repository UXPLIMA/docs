---
title: The player count
order: 303
description: Real, fixed or one short of full, and the offset.
icon: users
---

`players` in [config.conf](../config/config-conf.md#players) decides the two numbers on the right of the entry.

| `mode` | Online | Maximum |
|---|---|---|
| `real` | What the server reports | What the server reports |
| `fixed` | What the server reports | Always `players.maximum` (2026) |
| `just-one-more` | What the server reports | The shown count plus one, so the list never says the server is full |

`offset` (0) is added to the online count first. It may be negative; the count never goes below zero. The mode reads the
number after the offset, so `just-one-more` with an offset of 10 and four real players says 14 of 15.

The two numbers are what `<online>` and `<max>` read in the lines and the hover.

Related: [The hover list](hover.md)
