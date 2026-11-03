---
title: Custom items
order: 903
description: Naming an Oraxen, Nexo, ItemsAdder or CraftEngine item in a crate, reward or key file.
icon: gem
---

Wherever a content file takes a `material`, it may name a custom item with its plugin's prefix:

```hocon
items = [ { material = "oraxen:ruby", amount = 3 } ]
icon  { material = "nexo:crown" }
```

| Plugin | Example |
|---|---|
| Oraxen | `oraxen:ruby` |
| Nexo | `nexo:crown` |
| ItemsAdder | `itemsadder:namespace:item` |
| CraftEngine | `craftengine:namespace:item` |

The item is built by its own plugin, so its model, name and data are the ones it defines. A server with none of these
plugins answers nothing, and the line is reported in the console.

A key item and an icon may also take `item-model` and `model-data` for a resource pack of your own, with a vanilla
material.

Related: [Reward files](../config/reward-files.md), [Key files](../config/key-files.md)
