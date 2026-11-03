---
title: config.conf
order: 501
description: Every key of the main file, with its shipped value and what it does.
icon: file-cog
---

The main settings file. Every key is shown with the value the plugin ships. `/uxmdailyrewards reload` applies every
change except those marked as needing a restart.

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
| `file` | `"dailyrewards.db"` | The SQLite file, inside `data/`. Read only for `sqlite` |
| `host` | `"localhost"` | The database server, for the other three types |
| `port` | `0` | Its port; `0` is the type's own, 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `database` | `"minecraft"` | The database name |
| `username` | `"minecraft"` | The user |
| `password` | `""` | The password |
| `table-prefix` | `"uxmdailyrewards_"` | Written in front of every table, so two plugins, or two copies of one, can share a database. Letters, digits and underscores |
| `pool-size` | `4` | How many connections the pool may open; SQLite always uses one |
| `local-only` | `false` | This one server keeps its rows in the SQLite file whatever `type` says, for a server of a network that has no business in the shared database |
| `copy-from-file` | `false` | Copies the rows of the SQLite file into the empty tables of the new database once, at the start |

The storage is opened once: a change needs a restart. See [Database](../database/overview.md).

## claim

| Key | Default | What it does |
|---|---|---|
| `window` | `"24h"` | How long one claim holds a player off |
| `grace` | `"24h"` | How much longer the streak survives after the window |

## calendar

| Key | Default | What it does |
|---|---|---|
| `length` | `28` | Days in one round, each a file in `days/` |

See [The calendar](../rewards/calendar.md).

## ranks

| Key | Default | What it does |
|---|---|---|
| `source` | `"auto"` | `auto`, `luckperms` or `permission`. Needs a restart |
| `permission-prefix` | `"uxmdailyrewards.rank."` | The node of a rank on the permission path, before the rank id |
| `ladder` | `["default", "vip", "mvp"]` | The ranks that count, worst first |

See [Ranks](../rewards/ranks.md).

## reminder

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Remind a player who joins with a day ready |
| `delay` | `"5s"` | How long after the join |
| `effects` | a title and subtitle, an action bar line, a pling | What the reminder does |

## economy

| Key | Default | What it does |
|---|---|---|
| `treasury-wait-ms` | `250` | How long one Treasury payment may take before it is given up and pays nothing; more than `0`. Reaches Treasury alone. Needs a restart |

## celebration

| Key | Default | What it does |
|---|---|---|
| `actions` | the level up sound, totem particles | Runs after every claim, on top of the day's reward |

See [Reminder and celebration](../rewards/reminder-and-celebration.md).

## menu

| Key | Default | What it does |
|---|---|---|
| `sounds.click` | `block.note_block.pling`, volume `0.6`, pitch `1.5` | A button that does something, when its own actions play no sound |
| `sounds.page` | `item.book.page_turn`, `0.7`, `1.0` | A page arrow |
| `sounds.denied` | `block.note_block.bass`, `0.6`, `0.9` | A click that is refused, such as on a day that is not ready |

Each sound is `{ name, volume, pitch }`; an empty `name` silences it.

Related: [Configuration](index.md), [Day files](../rewards/day-files.md)
