---
title: items.conf
order: 606
description: The vanilla item under an item model the plugin cannot place.
icon: sword
---

```hocon
answers {
  "myplugin:ruby_sword" = "diamond_sword"
}
```

| Key | Meaning |
|---|---|
| `answers` | Each key an item model (`namespace:path`), each value the vanilla item under it. A value without a namespace is a `minecraft:` item |

Most servers never need this file: the base item is read from the packs or learned the first time a player meets an
item with that model. `/uxmbedrock items unknown` lists the models still missing; give them here, reload and build.
