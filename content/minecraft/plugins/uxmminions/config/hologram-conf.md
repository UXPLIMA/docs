---
title: hologram.conf
order: 602
description: The hologram over a minion.
icon: type
---

| Key | Default |
|---|---|
| `enabled` | true |
| `offset-y` | 2.2 |
| `view-distance` | 24 |
| `update-interval-ticks` | 20 |
| `lines` | `@minion.hologram.name`, `@minion.hologram.level`, `@minion.hologram.status`, `@minion.hologram.storage` |
| `appearance` | `{}`: the shared hologram table |

The four lines read the minion's name, `Level <n>`, its status and `<used>/<capacity>`. Every line is a key of the
messages, so a translator never opens this file. See [Drawing](../features/drawing.md#the-hologram).

Related: [Drawing](../features/drawing.md)
