---
title: config.conf
order: 601
description: Every block and key of config.conf, with its default and what it does.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers, and the fallback for a missing line |
| `follow-client` | `true` | A player who chose nothing reads their client's language |
| `force` | `""` | One language for everybody. Empty for none |

See [Languages](languages.md).

## storage

| Key | Default | Meaning |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql`. Restart to change |
| `file` | `"crates.db"` | The SQLite file, inside `data/` |
| `host`, `port` | `"localhost"`, `0` | The database server; port `0` is 3306 or 5432 |
| `database`, `username`, `password` | `"minecraft"`, `"minecraft"`, `""` | The database and its user |
| `table-prefix` | `"uxmcrates_"` | Written in front of every table. Letters, digits and underscores |
| `pool-size` | `4` | Connections; SQLite uses one |
| `local-only` | `false` | This server keeps its rows in its own SQLite file, whatever `type` says |
| `copy-from-file` | `false` | Copies the SQLite rows into the empty tables of the new database once, at the start |

See [Database](../database/overview.md).

## idle

| Key | Default | Meaning |
|---|---|---|
| `look-for-viewers-ms` | `1000` | How often a crate with an idle effect asks who is near |
| `view-distance` | `24` | How near, in blocks, a player must be for the idle effect to be drawn |

## odds

| Key | Default | Meaning |
|---|---|---|
| `show` | `true` | The preview shows the weight and the reader's chance. Off: only you know the table. The reel stays faithful either way |
| `decimals` | `2` | Decimal places of a chance, in the preview and `%uxmcrates_chance_...%` |

## pushback

| Key | Default | Meaning |
|---|---|---|
| `velocity` | `0.4` | How hard a refused player at a placed crate is pushed back. `0` for none |
| `actions` | `["[sound] block.note_block.bass 1 1"]` | Effect lines run on a refusal, in order |

An old `sound` key beside the list is folded into the front of it.

### Effect lines

Every list of actions in uxmCrates (pushback, a crate's effects, a rarity's effects, a key's clicks, an envoy's lists)
reads one grammar:

| Verb | Payload |
|---|---|
| `[message]` | A line to the player, MiniMessage, or `@key` for a messages line |
| `[broadcast]` | A line to everyone |
| `[actionbar]` | The action bar |
| `[title]`, `[subtitle]` | The title and subtitle |
| `[bossbar]` | A boss bar |
| `[sound]` | A sound key, volume and pitch: `block.chest.open 1 1` |
| `[particle]` | A particle |
| `[effect]` | A potion effect |
| `[console]` | A command as the console |
| `[player]` | A command as the player |
| `[close]` | Closes the player's window |
| `[take-money]`, `[give-money]` | A currency and an amount: `vault 500` |
| `[take-item]` | A material and an amount: `DIAMOND 3` |

Between the verb and the payload a line may carry `{delay=, chance=, repeat=, every=, if=}`:
`"[broadcast] {delay=200} @envoy.drop.half"`. A cost line takes no block.

## claims

| Key | Default | Meaning |
|---|---|---|
| `place-inside-others-claims` | `false` | An operator may place a crate inside another player's claim |
| `open-inside-others-claims` | `true` | A player may open a crate inside another player's claim |
| `providers` | `{}` | Turn a claim plugin off by name: `{ lands = false }` |
| `combine` | `"any-land"` | `any-land` or `all-land`: how overlapping claims are folded |

See [Claims](../features/claims.md).

## log

| Key | Default | Meaning |
|---|---|---|
| `to-database` | `true` | Every win is a row, read by `/uxmcrates log` |
| `to-file` | `true` | Every win is also a line in a file |
| `file` | `"logs/crates-%date%.log"` | Under `data/`. `%date%` starts a new file each day |
| `page-size` | `45` | Rows per page of the log window |

## placeholders

| Key | Default | Meaning |
|---|---|---|
| `refresh-seconds` | `5` | How often the copy that answers placeholders is read again, per online player |

## leaderboard

| Key | Default | Meaning |
|---|---|---|
| `size` | `10` | Places each board keeps |
| `refresh-seconds` | `15` | How often the boards are read. Restart to change |
| `empty-head` | `"entity/player/wide/steve"` | The head of a place nobody holds: a texture path, a name or an id |
| `periods` | `[alltime, monthly, weekly, daily]` | The boards kept. Empty keeps all four |
| `time-zone` | `""` | The zone a day starts in, such as `"Europe/Istanbul"`. Empty is the server's |
| `week-starts` | `monday` | The first day of a week |
| `keep-windows` | `12` | Past windows of a period kept, so last week's winner can be read |

## date-format and ledger

| Key | Default | Meaning |
|---|---|---|
| `date-format` | `"yyyy-MM-dd HH:mm"` | How a moment is written in the ledger and the log, as a Java date pattern |
| `ledger.page-size` | `45` | Rows per page of `/uxmcrates key ledger` |

## broadcast

| Key | Default | Meaning |
|---|---|---|
| `opt-out-permission` | `"uxmcrates.broadcast.hide"` | A player holding it does not read win broadcasts. It defaults to true for everyone: see [Permissions](../permissions/reference.md) |

## rerolls

| Key | Default | Meaning |
|---|---|---|
| `price.currency` | `""` | The currency a bought reroll is paid in, named like a cost |
| `price.amount` | `""` | An expression over `reroll`, the number of the bought reroll: `"reroll * 250"` |
| `price.maximum` | `0` | Rerolls that may be bought in one opening. `0` is no cap |
| `groups.<name>.permission` | | The node that puts a player on the group |
| `groups.<name>.amount` | | Rerolls per opening |
| `groups.<name>.priority` | | The highest priority a player holds wins; groups do not stack |
| `groups.<name>.refusal` | | The line shown to a player below this group |

Ships with `silver` (`uxmcrates.reroll.silver`, 1, priority 1) and `gold` (`uxmcrates.reroll.gold`, 3, priority 2).
See [Rerolls](../features/rerolls-and-milestones.md#rerolls).

## milestones

| Key | Meaning |
|---|---|
| `per-crate."<count>"` | A rung reached by that many openings of one crate |
| `every-crate."<count>"` | A rung reached by that many openings of any crate |
| `.name` | The rung's name, in the three shapes a crate name takes |
| `.items`, `.commands` | What it gives |
| `.automatic` | `true` hands it over at once; `false` waits in the milestones window |

Ships with per-crate `10` (a diamond, automatic) and `100` (a netherite ingot), and every-crate `500` (a netherite
block).

## luck

| Key | Default | Meaning |
|---|---|---|
| `permission` | `{}` | `"<node>" = <multiplier>`: the weight of every reward is multiplied for a holder. The highest wins |

## purge

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `false` | Forget players who stopped coming |
| `after-days` | `180` | Days away before a player's rows go |
| `on-start` | `true` | One pass at start |
| `every-hours` | `12` | A pass this often; `0` turns the timer off |

## retention

| Key | Default | Meaning |
|---|---|---|
| `log-days` | `0` | Days of the opening log kept. `0` keeps all |
| `ledger-days` | `0` | Days of the key ledger kept. `0` keeps all |

## link-tool

| Key | Default | Meaning |
|---|---|---|
| `item.material` | `"BLAZE_ROD"` | The tool's item |
| `item.glow` | `true` | The glint |
| `item.name`, `item.lore` | `@content.config.link-tool.item...` | `<crate>` is the crate's name in the reader's language |

## editor

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Off: the editor's windows and input listeners are not registered and the command says it is off |

## menu

| Key | Default | Meaning |
|---|---|---|
| `sounds.click` | `block.note_block.pling`, 0.6, 1.5 | A button that does something |
| `sounds.page` | `item.book.page_turn`, 0.7, 1.0 | A page arrow |
| `sounds.denied` | `block.note_block.bass`, 0.6, 0.9 | A refused click |

Each is `{ name, volume, pitch }`; an empty name silences it.

Related: [opening.conf](opening-conf.md), [economy.conf](economy-conf.md)
