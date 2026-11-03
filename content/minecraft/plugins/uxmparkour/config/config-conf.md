---
title: config.conf
order: 501
description: Every key of the main file, with its shipped value and what it does.
icon: file-cog
---

The main settings file. Every key is shown with the value the plugin ships. `/uxmparkour reload` applies a change
unless the key says otherwise.

## language

| Key | Default | What it does |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers, and the fallback for a line a translation has not reached |
| `follow-client` | `true` | A player who has chosen nothing reads the language their client is set to; `false` starts everyone in `default` |
| `force` | `""` | One language for everybody, whatever they chose and whatever their client says; empty for none |

The languages are the files in `messages/`; nothing here lists them. See [Languages](languages.md).

## storage

| Key | Default | What it does |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql` |
| `file` | `"parkour.db"` | The SQLite file, inside `data/`. Read only for `sqlite` |
| `host` | `"localhost"` | The database server, for the other three types |
| `port` | `0` | Its port; `0` is the type's own, 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `database` | `"minecraft"` | The database name |
| `username` | `"minecraft"` | The user |
| `password` | `""` | The password |
| `table-prefix` | `"uxmparkour_"` | Written in front of every table, so two plugins, or two copies of one, can share a database. Letters, digits and underscores |
| `pool-size` | `4` | How many connections the pool may open; SQLite always uses one |
| `sync-seconds` | `2` | How often this server reads what other servers on the same database changed for a player who is here; `0` turns it off. SQLite reads nothing |
| `local-only` | `false` | This one server keeps its rows in the SQLite file whatever `type` says, for a server of a network that has no business in the shared database |
| `copy-from-file` | `false` | Copies the rows of the SQLite file into the empty tables of the new database once, at the start |

The storage is opened once: a change needs a restart. See [Database](../database/index.md) for each backend and
for moving between them.

## run

| Key | Default | What it does |
|---|---|---|
| `blocks-ahead` | `4` | How many blocks stand in front of the player. More lets them see further, at the cost of more blocks per runner |
| `blocks-behind` | `2` | How many blocks stay behind before they vanish; `0` removes a block the moment the player leaves it |
| `max-fall` | `6` | How far below the block last stood on a player may fall before the run ends, at least 1 |
| `default-difficulty` | `"normal"` | The difficulty of a new arena, and of a player in an arena whose difficulty no longer exists. Must be one in `difficulties` |
| `default-theme` | `"stone"` | The theme of a new arena, and of a player in an arena whose theme no longer exists. Must be one in `themes` |
| `protect-arena` | `true` | Nobody digs or builds inside an arena's box while a run is in it; `uxmparkour.admin.build` is never stopped |
| `announce-seed` | `false` | Tells a player the seed their course was generated from |

## leaderboard

| Key | Default | What it does |
|---|---|---|
| `size` | `10` | How many places each board keeps; a place past this names nobody |
| `refresh-seconds` | `15` | How often the boards are read from the database. A finished run shows at the next read. Needs a restart |
| `empty-head` | `"entity/player/wide/steve"` | What `%uxmparkour_top_<board>_<period>_<n>_head%` gives for a place nobody holds: a texture path, a player's name or id |
| `periods` | `[ alltime, monthly, weekly, daily ]` | The periods each board is kept for; an empty list keeps all four |
| `time-zone` | `""` | The zone a day starts in, such as `"Europe/Istanbul"`; empty is the server's. Servers sharing a database should write the same |
| `week-starts` | `monday` | The day a week starts on |
| `keep-windows` | `12` | How many ended windows of a period are kept, so last week's winner can still be read; older ones are deleted |

The boards are `best`, `jumps` and `runs`. See [Placeholders](../placeholders/reference.md#boards).

## difficulties

One block per difficulty, the id on the left:

| Key | What it does |
|---|---|
| `min-forward`, `max-forward` | The gap along the heading, at least 1 |
| `max-sideways` | How far to either side, `0` for a straight line |
| `min-rise`, `max-rise` | The height change, negative for down |

Ships `easy`, `normal` and `hard`. Jumps a player cannot make are dropped; a difficulty with none left stops the
reload. The list is yours: an update never puts back one you removed. See [Difficulties](../arenas/difficulties.md).

## themes

One block per theme, the id on the left:

| Key | What it does |
|---|---|
| `icon` | The item shown for it in the theme picker; the first material when left out |
| `materials` | The blocks drawn from, at least one |

Ships `stone`, `wood`, `quartz`, `glow` and `wool`. See [Themes](../arenas/themes.md).

## effects

Five lists of lines, one per moment: `fell`, `exhausted`, `milestone`, `broadcast` and `personal-best`. Each line is
`[verb] payload`, run in order; an empty list is silent. See [Moments](../arenas/moments.md) for the verbs, the
`{delay, repeat, every, if, chance}` block and the values each moment reads.

## milestones

One block per milestone, named by its jump count in quotes:

| Key | What it does |
|---|---|
| `money` | Lines of `<economy> <amount>`: `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>`, `treasury[:<currency>]` |
| `broadcast` | `true` plays the `broadcast` moment for everyone |

Ships 25, 50, 100 and 250 jumps paying experience. The list is yours. See [Rewards](../arenas/rewards.md).

## economy

| Key | Default | What it does |
|---|---|---|
| `treasury-wait-ms` | `250` | How long one Treasury call may take before nothing is paid. It runs on the player's thread; only Treasury reads it |

## session-items

Two blocks, `restart` and `leave`:

| Key | restart | leave | What it does |
|---|---|---|---|
| `enabled` | `true` | `true` | Whether the item is given |
| `slot` | `3` | `5` | The inventory slot, `0` to `8` for the hotbar. The item is recognised by its slot |
| `material` | `"CLOCK"` | `"RED_DYE"` | The item |

The names and lore are `item.restart` and `item.leave` in the messages files. See
[Runs](../arenas/runs.md#the-session-items).

## feedback

Three cues that fire several times a second, each with a `sound` and a `particles` block:

| Cue | Fires | Sound | Particles |
|---|---|---|---|
| `jump` | On every landing on the next block | on: `entity.experience_orb.pickup` | on: `crit` |
| `block-placed` | On every block that appears | off: `block.amethyst_block.place` | on: `end_rod` |
| `block-removed` | On every block that vanishes | off: `block.stone.break` | on: `smoke` |

| Sound key | What it does |
|---|---|
| `enabled` | `false` silences it |
| `key` | A vanilla sound key, such as `minecraft:entity.experience_orb.pickup` |
| `volume`, `pitch` | The volume and starting pitch; the jump ships at 0.6 and 0.8 |
| `pitch-step` | How much the pitch climbs per jump of a streak, so a streak is audible; `0` for a flat sound. The jump ships at 0.02 |
| `max-pitch` | Where the climb stops, 2.0 |

| Particle key | What it does |
|---|---|
| `enabled` | `false` silences it |
| `key` | A particle name, which may carry data after a space: `"minecraft:dust #ff0000"`, `"minecraft:block oak_log"` |
| `count`, `spread`, `speed` | How many, how far apart, how fast |

A sound or particle the server does not know is skipped with a line in the console.

## menu

| Key | Default | What it does |
|---|---|---|
| `sounds.click` | `block.note_block.pling`, 0.6, 1.5 | A button that does something |
| `sounds.page` | `item.book.page_turn`, 0.7, 1.0 | A page arrow |
| `sounds.denied` | `block.note_block.bass`, 0.6, 0.9 | A button the player may not use |

Each is `{ name = "...", volume = ..., pitch = ... }`, named by its vanilla key. An empty name silences one. A button
whose own actions play a sound in its window file plays that sound alone.

Related: [Configuration](index.md), [arenas.conf](arenas-conf.md), [Messages](messages.md)
