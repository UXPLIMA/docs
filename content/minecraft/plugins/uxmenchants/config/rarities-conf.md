---
title: rarities.conf
order: 502
description: The three shipped rarities, every key of a rarity, and chances that scale with the level.
icon: gem
---

A rarity is an acquisition weight and nothing else: how likely an enchantment of it is on each route, and the table
level it starts at. How it is drawn is the type.

| Key | Meaning |
|---|---|
| `name` | What a player reads; one line, one per language, or `@key` |
| `order` | Where it sorts in a window and a lore block; lowest first |
| `minimum-level` | The table level it starts being offered at |
| `table-chance` | Out of a hundred, at the table |
| `villager-chance` | Out of a hundred, in a villager trade |
| `loot-chance` | Out of a hundred, in a loot chest |
| `fishing-chance`, `mob-drop-chance`, `raid-chance` | Optional; default to `loot-chance` |

A chance is a number or a [modifier block](../features/numbers-and-descriptions.md) read at the level being rolled. `0`
closes that route for that rarity without closing it for the rest.

## Shipped

| Rarity | `order` | `minimum-level` | `table-chance` | `villager-chance` | `loot-chance` |
|---|---|---|---|---|---|
| `common` | 1 | 1 | 12 | 8 | 10 |
| `rare` | 2 | 10 | 3, plus 0.4 per level, capped at 9 | 3 | 4 |
| `legendary` | 3 | 25 | 0.5, plus 0.2 on a diminishing curve, capped at 2 | 0.5 | 1 |

Every route but the table rolls at level 1, so `rare` and `legendary` are reached only at the table; see
[Acquisition](../features/acquisition.md#the-other-routes-roll-at-level-one).

When this file is wrong the whole reload is refused, naming the file and the line.

Related: [Acquisition](../features/acquisition.md)
