---
title: config.conf
order: 601
description: Every key of the main file, with its shipped value and what it does.
icon: file-cog
---

The main settings file. Every key is shown with the value the plugin ships.

## language

| Key | Default | What it does |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers, and the fallback for a line a translation has not reached |
| `follow-client` | `true` | A player who has chosen nothing reads the language their client is set to; `false` starts everyone in `default` |
| `force` | `""` | One language for everybody; empty for none |

## storage

| Key | Default | What it does |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql` |
| `file` | `"levels.db"` | The SQLite file, inside `data/` |
| `host` | `"localhost"` | The database server, for the other three types |
| `port` | `0` | Its port; `0` is 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `database`, `username`, `password` | `"minecraft"`, `"minecraft"`, `""` | The database and its user |
| `table-prefix` | `"uxmlevels_"` | Written in front of every table |
| `pool-size` | `4` | Connections the pool may open; SQLite uses one |
| `sync-seconds` | `2` | How often this server reads what other servers on the same database changed for a player who is here; `0` turns it off |
| `local-only` | `false` | This one server keeps its rows in the SQLite file whatever `type` says |
| `copy-from-file` | `false` | Copies the SQLite rows into the empty tables of the new database once, at the start |

Needs a restart. See [Database](../database/overview.md).

## economy

| Key | Default | What it does |
|---|---|---|
| `currency` | `"exp"` | The balance a prestige's `money-cost` is charged against: `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>`, `treasury` |
| `treasury-wait-ms` | `250` | How long one Treasury call may take |

Needs a restart. See [Economies](../integrations/economies.md).

## experience

| Key | Default | What it does |
|---|---|---|
| `playtime-interval-ms` | `60000` | How often `playtime` events pay; the amount is per interval |
| `idle-timeout-ms` | `300000` | How long a player may be still before playtime stops paying; `0` turns it off |
| `hourly-cap` | `0` | The most one player may earn on one track in a rolling hour; `0` for none |
| `global-multiplier` | `1.0` | What every earned amount is multiplied by first |
| `permission-multipliers` | `vip` 1.25, `mvp` 1.5 | A node and its multiplier; the best one held wins |

See [Multipliers and boosts](../tracks/multipliers-and-boosts.md) and [Anti-abuse](../tracks/anti-abuse.md).

## anti-abuse

| Key | Default | What it does |
|---|---|---|
| `mark-player-placed-blocks` | `true` | A block a player places pays nothing when broken |
| `marks-per-chunk` | `4096` | Marks one chunk keeps; the oldest goes when full |

## leaderboard

| Key | Default | What it does |
|---|---|---|
| `size` | `100` | Places a board holds |
| `refresh-seconds` | `120` | How often the boards are read |
| `chat-lines` | `10` | Places `/levels top` prints |
| `empty-head` | `"entity/player/wide/steve"` | The head of an empty place |
| `periods` | `[ alltime, monthly, weekly, daily ]` | The periods kept |
| `time-zone` | `""` | The zone a day starts in; empty is the server's |
| `week-starts` | `monday` | The first day of a week |
| `keep-windows` | `12` | Past periods kept |

See [Boards](../tracks/boards.md).

## hud

| Key | Default | What it does |
|---|---|---|
| `refresh-ms` | `1000` | How often the heads-up surfaces are drawn |
| `gain-notice-ms` | `3000` | How long a gain holds the action bar |

See [Heads-up](../tracks/heads-up.md).

## prestige

| Key | Default | What it does |
|---|---|---|
| `required-level` | `100` | The level needed; a shorter track uses its own maximum |
| `money-cost` | `0.0` | The price in `economy.currency` |
| `experience-cost` | `0` | Experience the player must hold |
| `maximum` | `10` | Prestiges per track |
| `bonus-per-prestige` | `0.1` | Added to the multiplier per prestige |
| `default-badge` | `"<warn>[<prestige>]"` | The badge for a count not named below |
| `badges` | `"10" = "<warn>[X]"` | A badge per exact count |

See [Prestige](../tracks/prestige.md).

## menu

| Key | Default | What it does |
|---|---|---|
| `sounds.click` | `block.note_block.pling`, `0.6`, `1.5` | A button that does something, when its own actions play no sound |
| `sounds.page` | `item.book.page_turn`, `0.7`, `1.0` | A page arrow |
| `sounds.denied` | `block.note_block.bass`, `0.6`, `0.9` | A refused click |

Related: [Configuration](index.md), [Track files](track-files.md)
