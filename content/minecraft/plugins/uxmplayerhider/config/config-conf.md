---
title: config.conf
order: 401
description: Every key of the main file, with its shipped value and what it does.
icon: file-cog
---

The main settings file. Every key is shown with the value the plugin ships. `/uxmplayerhider reload` applies every
change except `storage`.

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
| `file` | `"playerhider.db"` | The SQLite file, inside `data/`. Read only for `sqlite` |
| `host` | `"localhost"` | The database server, for the other three types |
| `port` | `0` | Its port; `0` is the type's own, 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `database` | `"minecraft"` | The database name |
| `username` | `"minecraft"` | The user |
| `password` | `""` | The password |
| `table-prefix` | `"uxmplayerhider_"` | Written in front of every table, so two plugins, or two copies of one, can share a database. Letters, digits and underscores |
| `pool-size` | `4` | How many connections the pool may open; SQLite always uses one |
| `sync-seconds` | `2` | How often this server reads what other servers on the same database changed for a player who is here: the state they chose; `0` turns it off. SQLite reads nothing |
| `local-only` | `false` | This one server keeps its rows in the SQLite file whatever `type` says, for a server of a network that has no business in the shared database |
| `copy-from-file` | `false` | Copies the rows of the SQLite file into the empty tables of the new database once, at the start |

The storage is opened once: a change needs a restart. See [Database](../database/overview.md) for each backend and
for moving between them.

## worlds

| Key | Default | What it does |
|---|---|---|
| `worlds` | `[]` | The worlds the tool and the command work in, without regard to case; empty is every world |

See [Worlds and cooldown](../hiding/worlds-and-cooldown.md#worlds).

## cooldown

| Key | Default | What it does |
|---|---|---|
| `ms` | `2000` | Milliseconds between two changes; `0` turns it off. `uxmplayerhider.cooldown.bypass` never waits |

## tool

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Hand out the tool |
| `slot` | `7` | The hotbar slot, `0` to `8` |
| `lock-in-slot` | `true` | The tool cannot be moved, dropped, swapped or placed, and never drops on death |
| `give-on-join` | `true` | Hand it out on join |
| `give-on-respawn` | `true` | Hand it out on respawn |
| `give-on-world-change` | `true` | Hand it out on a world change |
| `select-on-give` | `false` | Make it the held item when it is handed out |
| `icons.all` | `LIME_DYE`, no glow | The icon of `all`: `material`, `glow`, `custom-model-data` |
| `icons.important` | `YELLOW_DYE`, no glow | The icon of `important` |
| `icons.none` | `GRAY_DYE`, glowing | The icon of `none` |

See [The tool](../hiding/tool.md).

## feedback

| Key | Default | What it does |
|---|---|---|
| `effects.all` | a subtitle, an action bar line, a pling | Runs when a player moves into `all` |
| `effects.important` | the same for `important` | Runs when a player moves into `important` |
| `effects.none` | the same for `none`, with a bass note | Runs when a player moves into `none` |

The lines, and how one is written, are on [Effects](../hiding/effects.md).

Related: [Configuration](index.md), [Messages](messages.md)
