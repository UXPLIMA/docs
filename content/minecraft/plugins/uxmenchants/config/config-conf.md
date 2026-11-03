---
title: config.conf
order: 601
description: Every block and key of config.conf, with its default and what it does.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `"en"` | The language every message falls back to |
| `follow-client` | true | Use the player's client language when there is a file for it |
| `force` | `""` | One language for the whole server; empty for none |

## storage

| Key | Default | Meaning |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql` |
| `file` | `"data.db"` | The SQLite file, inside `data/` |
| `host`, `port`, `database`, `username`, `password` | `localhost`, 0, `minecraft`, `minecraft`, `""` | The database server; port 0 is the default (3306 or 5432) |
| `table-prefix` | `"uxmenchants_"` | Before every table name |
| `pool-size` | 4 | Connections; SQLite uses one |
| `sync-seconds` | 2 | How often a server reads what others changed for its players (the description toggle); 0 is off |
| `local-only` | false | This one server keeps its rows on the SQLite file |
| `copy-from-file` | false | Copy the SQLite rows into an empty network database on the next start |

See [Database](../database/overview.md). A change needs a restart.

## item

| Key | Default | Meaning |
|---|---|---|
| `namespace` | `"uxmenchants"` | The namespace of the key we write under |
| `key` | `"enchantments"` | The container every enchantment on an item is written inside |
| `glint` | true | Whether an item with ours shimmers |
| `book-material` | `"ENCHANTED_BOOK"` | What a book is made of |

Change the namespace or the key and every item already enchanted stops being read: change them once, before the server
opens, or not at all.

## limit

| Key | Default | Meaning |
|---|---|---|
| `base` | 3 | How many of ours one item may carry |
| `node-prefix` | `"uxmenchants.limit."` | The node that raises it: `uxmenchants.limit.6` |
| `maximum-from-node` | 32 | The highest number a node may grant |

See [Applying](../features/applying.md#the-limit).

## acquisition

`table`, `villager`, `loot`, `fishing`, `mob-drops` and `raids` each take `enabled`, `cap` and `reduction`; `anvil`
takes `enabled` only.

| Route | `enabled` | `cap` | `reduction` |
|---|---|---|---|
| `table` | true | 2 | 3.0 |
| `villager` | true | 1 | 2.0 |
| `loot` | true | 2 | 2.5 |
| `anvil` | true | | |
| `fishing` | false | 1 | 2.5 |
| `mob-drops` | false | 1 | 2.5 |
| `raids` | false | 2 | 2.0 |

See [Acquisition](../features/acquisition.md).

## drag-and-drop

| Key | Default | Meaning |
|---|---|---|
| `enabled` | false | A left click with a book applies it |
| `cost` | 5 | Experience levels per application |
| `per-level` | true | Times the book's level |

## anvil

| Key | Default | Meaning |
|---|---|---|
| `enabled` | true | The anvil applies ours at all |
| `vanilla-costs` | false | Keep the server's own arithmetic |
| `base-cost` | 1 | Levels for the first of ours |
| `cost-exponent` | 1.1 | What each further one multiplies by |
| `overpowering` | false | Two at the maximum may go one past it, up to `overpower-max` |

See [Anvil and books](../features/anvil-and-books.md).

## grindstone

`enabled` (true): whether ours come off at all. Which ones is the type's rule.

## worlds

`mode` (`"blacklist"` or `"whitelist"`) and `list` (`[]`). See [Protection](../features/protection.md#worlds).

## books

| Key | Default | Meaning |
|---|---|---|
| `name-format` | `"<accent><name> <roman>"` | The name; `<name>`, `<roman>`, `<number>` |
| `lore` | `["", "@book.hint"]` | Under the description; `@key` is a line of `messages/` |
| `glow` | true | Whether the book shimmers |

## display

| Key | Default |
|---|---|
| `enabled` | true |
| `description-threshold` | 4 |
| `wrap-width` | 38 |
| `descriptions-on-by-default` | true |
| `description-format` | `"<subtext><line>"` |
| `not-met-format` | `"<dim><st><name> <roman></st>"` |
| `collapse.threshold`, `per-line`, `delimiter` | 6, 3, `", "` |
| `order` | `["type", "rarity", "name"]` |

See [Display](../features/display.md).

## charges

| Key | Default |
|---|---|
| `enabled` | false |
| `maximum` | 500 |
| `consume` | 1 |
| `recharge` | 50 |
| `fuel-material` | `"GLOWSTONE_DUST"` |
| `bands` | 50 good, 20 level, 0 bad |

See [Charges and cooldowns](../features/charges-and-cooldowns.md).

## cooldowns

`persist` (false): keep cooldowns in the database across a restart.

## block-break-treaty

`enabled` (true) and `metadata-key` (`"blockbreakevent-ignore"`). See [Protection](../features/protection.md).

## effects

`materials.whitelist` (`[]`) and `materials.blacklist` (`BEDROCK`, `BARRIER`, `END_PORTAL_FRAME`, `SPAWNER`,
`REINFORCED_DEEPSLATE`).

## claims

`combine` (`"any-land"` or `"all-land"`) and `providers`, nineteen switches all true: `worldguard`, `griefprevention`,
`lands`, `towny`, `factions`, `residence`, `plotsquared`, `huskclaims`, `husktowns`, `kingdoms`, `bentobox`,
`superiorskyblock`, `griefdefender`, `simpleclaimsystem`, `excellentclaims`, `homestead`, `rclaim`, `xclaim`,
`uxmclaims`.

## editor

`enabled` (true). Off: no editor windows, no input listener, and the command says so. Needs a restart.

## menu

`menu.sounds`: `click` (`block.note_block.pling` 0.6 1.5), `page` (`item.book.page_turn` 0.7 1.0) and `denied`
(`block.note_block.bass` 0.6 0.9), each `name`, `volume`, `pitch`. An empty name silences one. A button whose own
actions play a sound plays that sound alone.

Related: [Configuration](index.md)
