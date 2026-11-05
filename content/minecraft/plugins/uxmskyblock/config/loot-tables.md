---
title: Loot tables
order: 691
description: "One file per table: loot/<name>.conf, shown with treasure.conf as shipped."
icon: gift
---

A loot table.

Every file in loot/ is a loot table, named by the file: this one is treasure. A game mode fills a chest, a barrel or any container from a table by its name, the first time a player opens it.

A table is rolled a number of times. Each roll lands on a rarity, by weight, then on one of that rarity's entries, by weight. A rarity can name an entry of modules/effects.conf, which fires for the player who finds it.

An entry is one of three things:

```text
item { ... }        an item, written as in any menu or shop file; amount is
                    a number or a span like "2-6", and slot puts it in that
                    slot when the slot is free
actions = [ ... ]   action lines, as in modules/effects.conf, run for the
                    player who finds it
vanilla = "..."     one of the server's own loot tables, the game's or a
                    datapack's, by its key
```

An entry with no rarity is common, and one with no weight weighs 1.

The file is `plugins/uxmSkyblock/loot/treasure.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `rolls` | `3-5` |  |
| `rarities.common.rare.entries` | a list of 8 entries, below |  |

`rarities.common.rare.entries`:

| # | `weight` | `amount` | `item.material` | `vanilla` | `rarity` | `item.stored-enchants.mending` | `slot` |
|---|---|---|---|---|---|---|---|
| 1 | `10` | `4-12` | `BREAD }` |  |  |  |  |
| 2 | `8` | `2-6` | `IRON_INGOT }` |  |  |  |  |
| 3 | `6` | `1-3` | `GOLD_INGOT }` |  |  |  |  |
| 4 | `4` |  |  | `minecraft:chests/simple_dungeon` |  |  |  |
| 5 | `5` | `1-2` | `DIAMOND }` |  | `rare` |  |  |
| 6 | `2` |  | `ENCHANTED_BOOK` |  | `rare` | `1 }` |  |
| 7 | `1` |  | `NETHERITE_SCRAP }` |  | `legendary` |  | `13` |
| 8 | `1` |  |  |  | `legendary` |  |  |
