---
title: config.conf
order: 401
description: Every key of the main file, with its shipped value and what it does.
icon: file-cog
---

The main settings file. Every key is shown with the value the plugin ships. `/uxmhubpvp reload` applies a change
unless the key says otherwise. The kit is in its own file, [kit.conf](kit-conf.md), and the wording of every line in
the [messages files](messages.md).

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
| `file` | `"hubpvp.db"` | The SQLite file, inside `data/`. Read only for `sqlite` |
| `host` | `"localhost"` | The database server, for the other three types |
| `port` | `0` | Its port; `0` is the type's own, 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `database` | `"minecraft"` | The database name |
| `username` | `"minecraft"` | The user |
| `password` | `""` | The password |
| `table-prefix` | `"uxmhubpvp_"` | Written in front of every table, so two plugins, or two copies of one, can share a database. Letters, digits and underscores |
| `pool-size` | `4` | How many connections the pool may open; SQLite always uses one |
| `sync-seconds` | `2` | How often this server reads what other servers on the same database changed for a player who is here: their kills, deaths and streaks; `0` turns it off. SQLite reads nothing |
| `local-only` | `false` | This one server keeps its rows in the SQLite file whatever `type` says, for a server of a network that has no business in the shared database |
| `copy-from-file` | `false` | Copies the rows of the SQLite file into the empty tables of the new database once, at the start |

The storage is opened once: a change needs a restart. See [Database](../database/index.md) for each backend and
for moving between them.

## arena

| Key | Default | What it does |
|---|---|---|
| `only-fighters-may-fight` | `true` | Refuses a hit unless both players are fighting. Turn it off only on a world whose own PvP rules decide who may fight |
| `hands-off-when` | `[]` | Placeholders that name another plugin's game; a hit between two players the same one names is left to that game. See [Other games](../integrations/other-games.md) |

## warm-up

| Key | Default | What it does |
|---|---|---|
| `enter-ms` | `3000` | How long turning PvP on takes, in milliseconds. `0` is at once. Not negative |
| `leave-ms` | `8000` | How long turning it off takes. Longer on purpose, so a losing player cannot step out at once |
| `move-limit` | `1.5` | Blocks a player may move from where they pressed before the warm-up stops. Not negative |
| `show-action-bar` | `true` | Counts the warm-up down on the action bar |

## combat-tag

| Key | Default | What it does |
|---|---|---|
| `duration-ms` | `10000` | How long a player stays tagged after a hit. `0` switches tagging off |
| `allowed-commands` | `["pvp", "msg", "r", "reply", "helpop"]` | The only commands a tagged player may type. A whitelist |

See [Combat tag](../fighting/combat-tag.md).

## toggle-item

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Hands out the toggle item. `false` hands out none |
| `give-on-join` | `true` | Puts it in the inventory on every join |
| `slot` | `2` | Its hotbar slot, `0` to `8`. An item already there stays, and the toggle takes the first free slot |
| `available-in-pvp` | `true` | Puts its leave face on top of the kit |
| `in-pvp.slot` | `8` | The slot of the leave face, `0` to `8`. Left out, it is `slot` |
| `in-pvp.item` | red dye, glowing | The leave face. Delete the `in-pvp` block to keep the lobby face in the fight |
| `item` | iron sword, unbreakable | The lobby face |

An item is written with the keys of a kit piece: `material`, `name`, `lore`, `amount`, `enchants`, `flags`,
`unbreakable`, `glow`, `hide-vanilla-tooltip` and `custom-model-data`. See [kit.conf](kit-conf.md#an-item). The
shipped faces write every line as `@item.join.*` and `@item.leave.*` keys of the messages files, so they read in
each player's language. See [Turning PvP on and off](../fighting/overview.md#the-toggle-item).

## respawn

| Key | Default | What it does |
|---|---|---|
| `instant` | `true` | Skips the death screen of a fighter |
| `spawn.world` | `""` | Where a dead fighter wakes up. Blank leaves it to the server or the lobby plugin |
| `spawn.x`, `y`, `z` | `0.5`, `64.0`, `0.5` | The point in that world |
| `spawn.yaw`, `pitch` | `0.0`, `0.0` | The facing |

## visuals

| Key | Default | What it does |
|---|---|---|
| `glow` | `true` | Outlines every fighter. The colour is the player's team colour, which uxmGlow sets; white without it |
| `hit-particles` | `true` | Draws particles on a fighter who is hit |
| `hit-particle` | `"CRIT"` | The particle, with its data after a space, such as `"dust #ff0000"` |
| `hit-particle-count` | `12` | How many, at least 1 |

## rewards

| Key | Default | What it does |
|---|---|---|
| `money` | `["exp 10"]` | What a kill pays, one line per balance. See [Rewards](../fighting/rewards.md) |

## economy

| Key | Default | What it does |
|---|---|---|
| `treasury-wait-ms` | `250` | How long one Treasury call may take before the payment is given up on. Above 0 |

## leaderboard

| Key | Default | What it does |
|---|---|---|
| `size` | `10` | How many places each board keeps; a place past this names nobody |
| `refresh-seconds` | `15` | How often the boards are read from the database. A kill shows at the next read. Needs a restart |
| `empty-head` | `"entity/player/wide/steve"` | What `%uxmhubpvp_top_<board>_<period>_<n>_head%` gives for a place nobody holds: a texture path, a player's name or id |
| `periods` | `[ alltime, monthly, weekly, daily ]` | The periods each board is kept for; an empty list keeps all four |
| `time-zone` | `""` | The zone a day starts in, such as `"Europe/Istanbul"`; empty is the server's. Servers sharing a database should write the same |
| `week-starts` | `monday` | The day a week starts on |
| `keep-windows` | `12` | How many ended windows of a period are kept, so last week's winner can still be read; older ones are deleted |

The boards are `kills` and `streak`. See [Placeholders](../placeholders/reference.md#boards).

## effects

The ten moments, each a list of action lines: `pvp-on`, `pvp-off`, `warm-up-entering`, `warm-up-leaving`,
`warm-up-cancelled`, `warm-up-moved`, `combat-tagged`, `kill`, `kill-paid` and `death`. A moment written `[]` is
silent; a moment left out of the file runs its shipped lines. See [Moments](../fighting/moments.md).

Related: [Configuration](index.md), [kit.conf](kit-conf.md), [Moments](../fighting/moments.md)
