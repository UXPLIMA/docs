---
title: boxes.conf
order: 703
description: Every key of a mystery box and its table.
icon: package-open
---

```hocon
boxes = [
  {
    id    = "common"
    tier  = "common"
    icon  = "CHEST"
    price = 1000.0
    rewards = [
      { cosmetic = "flame_trail", weight = 30 }
      { cosmetic = "cloud_trail", weight = 5, duplicate-coins = 300.0 }
    ]
  }
]
```

| Key | Meaning |
|---|---|
| `id` | The box's name, in its node and in a station |
| `tier` | `common`, `uncommon`, `rare`, `epic` or `legendary`: its colour |
| `icon` | The menu item |
| `price` | One opening's cost |
| `permission` | The node; default `uxmcosmetics.box.<id>` |
| `rewards` | The table |
| `rewards[].cosmetic` | A piece id from any family |
| `rewards[].weight` | Its share of the draw; `0` is never drawn |
| `rewards[].duplicate-coins` | What a duplicate pays; `0` or absent pays `economy.duplicate-refund-fraction` of the piece's price |

A box with no entry, or every weight 0, is refused when read and named in the console. The shipped boxes are listed on
[Mystery boxes](../features/mystery-boxes.md#the-shipped-boxes).

Related: [Mystery boxes](../features/mystery-boxes.md)
