---
title: Sources
order: 403
description: The price list, the seventeen source types, entry keys and categories.
icon: list
---

A source file is how a skill is earned: a `default` block merged into every entry, and a `sources` block of entries.
An entry always wins over the default.

## Types

`block-break`, `block-interact`, `block-collect`, `entity-kill`, `entity-damage`, `damage-taken`, `damage-blocked`,
`fishing`, `brewing`, `enchanting`, `anvil-use`, `grindstone-use`, `item-consume`, `potion-splash`, `movement-pulse`,
`ability-use`, `discovery`. The set is closed, so a misspelling is refused at start rather than paying nothing.

## Entry keys

| Key | Meaning |
|---|---|
| `type` | One of the above |
| `blocks` | The materials; `entities`, `items`, `motions` and `abilities` are the same key under a name that suits the type. `*` stands for any run of characters, `@name` is a category |
| `states` | Block states that must hold, such as `age = 7` |
| `after-states` | States checked one tick later, for a block that changes as it goes |
| `support` | The side the supporting block is on, so a cane cut in half pays both pieces |
| `tool` | The item filter: `materials`, `excluded`, `categories`, `name`, `lore`, `custom-model-data`, `potion` |
| `trigger` | `death` or `damage`, for a mob entry |
| `scale-with-health` | A payout on damage scaled by the mob's whole health |
| `experience` | The base amount, before every multiplier |
| `money` | A figure the plugin publishes and never spends, for a jobs plugin to read; uxmSkills pays nobody |
| `multiplier` | A formula over the numeric block states, so a payout follows a crop's growth |
| `chance` | How often the entry pays, 0.0 to 1.0 |
| `cooldown-ms` | The shortest gap between two payments of this entry to one player |

## Categories

Declared in `config.conf` under `categories`, or at the top of a source file:

| Category | Holds |
|---|---|
| `ores` | Every ore and ancient debris |
| `logs` | `*_LOG`, `*_WOOD` |
| `saplings` | `*_SAPLING` |
| `pickaxes`, `shovels`, `axes`, `hoes`, `swords` | The tools |

`/skills sources <skill>` shows a skill's price list in game, sortable in the window.

Related: [Source files](../config/content-files.md#sources)
