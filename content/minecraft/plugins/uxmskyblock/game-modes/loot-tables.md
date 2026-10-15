---
title: Loot Tables
order: 130
description: The tables in loot/ a game mode fills its chests, barrels and other containers from,
  with rolls, rarities, items, actions and the game's own tables.
icon: gift
---

Every file in `loot/` is a loot table, named by the file: `loot/treasure.conf` is `treasure`. A
game mode fills a chest, a barrel or any container from a table by its name, the first time a
player opens it. The plugin ships five:

| Table | Where it is used |
| --- | --- |
| `treasure` | OneBlock chests, and any phase or moment that names it |
| `skygrid`, `skygrid_nether`, `skygrid_end` | The chests of a SkyGrid grid, one per dimension |
| `deep_boss` | What whoever slays the Poseidon deep boss is given |

Add a file and its name can be named wherever a table is asked for: a OneBlock phase or moment, a
SkyGrid palette, the Poseidon boss, a TradeWinds find.

## A table

```hocon
rolls = "3-5"

rarities {
    common { weight = 80 }
    rare { weight = 18, effects = "loot-rare" }
    legendary { weight = 2, effects = "loot-legendary" }
}

entries = [
    { weight = 10, amount = "4-12", item { material = BREAD } }
    { weight = 4, vanilla = "minecraft:chests/simple_dungeon" }
    { rarity = rare, weight = 5, amount = "1-2", item { material = DIAMOND } }
    { rarity = legendary, weight = 1, actions = ["[message] @loot.treasure_found", "[give-money] 250"] }
]
```

A table is rolled `rolls` times, a number or a span like `"3-5"`. Each roll lands on a rarity, by
weight, then on one of that rarity's entries, by weight. A rarity can name an entry of
`modules/effects.conf` in `effects`, which fires for the player who finds it.

## An entry

An entry is one of three things:

| Entry | What it gives |
| --- | --- |
| `item { ... }` | An item, written as in any menu or shop file. `amount` is a number or a span, and `slot` puts it in that slot when the slot is free |
| `actions = [ ... ]` | Action lines, as in `modules/effects.conf`, run for the player who finds it |
| `vanilla = "..."` | One of the server's own loot tables, the game's or a data pack's, by its key |

An entry with no `rarity` is common, and one with no `weight` weighs 1.
