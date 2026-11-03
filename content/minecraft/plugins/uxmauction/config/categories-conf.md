---
title: categories.conf
order: 603
description: The keys of a category, patterns, and the fallback.
icon: folder
---

```hocon
categories {
  weapons {
    order = 10
    icon = "minecraft:diamond_sword"
    matches = [ "*_sword", "bow", "crossbow", "trident", "mace", "shield", "arrow" ]
  }
}
fallback = "misc"
```

| Key | Meaning |
|---|---|
| `order` | Where the tab sits, and who takes an item two would accept: the lowest wins |
| `icon` | The tab's item; a menu file may name a different one |
| `matches` | Item ids; `*` stands for any run of characters; no namespace reads as `minecraft:` |
| `fallback` | The category an item no tab accepts goes to; it must exist |

The shipped ten are in [Categories](../features/categories.md#shipped).

Related: [Categories](../features/categories.md)
