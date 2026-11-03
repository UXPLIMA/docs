---
title: Enchantment page
order: 404
description: Every fact about one enchantment on one tile.
icon: file-search
---

`/enchants info <enchantment>`, or a click on a tile, opens `menus/info.conf`: three rows titled with the enchantment's
name.

| Slot | Item | Click |
|---|---|---|
| 13 | Enchanted book: the page | |
| 18 | Arrow, back | The wiki |
| 26 | Barrier, close | |

The page reads: the name, the rarity, the description, then the type, the highest level, what it goes on, what it
refuses, what it needs, what it replaces, and yes or no for the enchanting table, villagers, loot chests and the anvil.

```hocon
lore = ["tile:5 @menu.info.page type maximum targets conflicts requires removes table villager loot anvil"]
```

Take a word off the line and the page draws one fact fewer. The words are in `messages/` under `menu.info.page`.

Related: [Wiki](wiki.md), [The shipped enchantments](../features/catalogue.md)
