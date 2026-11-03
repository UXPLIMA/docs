---
title: config.conf
order: 601
description: Every key of config.conf, with its default and what it controls.
icon: file-cog
---

Read on enable and on `/uxmglow reload`, and swapped in whole.

## language

| Path | Default | What it controls |
|---|---|---|
| `language.default` | `en` | What a player reads when nothing else answers |
| `language.follow-client` | `true` | Whether a player who chose nothing reads their client's language |
| `language.force` | empty | One language for everybody. Empty means none |

## storage

| Path | Default | What it controls |
|---|---|---|
| `storage.type` | `sqlite` | `sqlite`, `mysql`, `mariadb` or `postgresql`. Any other word stops the plugin |
| `storage.file` | `glow.db` | The SQLite file, in the plugin's `data/` folder. A file left in the plugin folder by an older version moves there on the first start |
| `storage.host` / `storage.port` | `localhost` / `0` | The database server. A port of 0 is the engine's own |
| `storage.database` | `minecraft` | The database on that server |
| `storage.username` / `storage.password` | `minecraft` / empty | The connection |
| `storage.table-prefix` | `uxmglow_` | Written in front of every table |
| `storage.pool-size` | `4` | Connections in the pool. A SQLite file is opened with one writer whatever this says |
| `storage.sync-seconds` | `2` | How often this server reads the colours other servers changed for a player who is here. Ignored for SQLite; 0 turns it off |
| `storage.local-only` | `false` | True keeps this one server on its SQLite file under the same prefix, whatever `storage.type` says |
| `storage.copy-from-file` | `false` | True copies the rows of the SQLite file into the empty tables of the network database at start, and the log says how many. Tables that already hold rows are left alone |

## cycle

| Path | Default | What it controls |
|---|---|---|
| `cycle.tick-ms` | `250` | How often the one shared animation task wakes; the finest step any cycle can get |
| `cycle.minimum-interval-ms` | `250` | The shortest step a player may ask for |
| `cycle.default-interval-ms` | `1000` | The step used when a player names no number |
| `cycle.maximum-colours` | `8` | How many colours one cycle may hold |

## effects

`set`, `cycling`, `cleared`, `restored` and `lapsed`: what each moment does, as action lines. See
[Moments](../features/moments.md).

## menu

| Path | Default | What it controls |
|---|---|---|
| `menu.sounds.click` | `block.note_block.pling`, 0.6, 1.5 | A button that does something |
| `menu.sounds.page` | `item.book.page_turn`, 0.7, 1.0 | A page arrow |
| `menu.sounds.denied` | `block.note_block.bass`, 0.6, 0.9 | A button the player may not use |

Each sound has a `name`, a `volume` and a `pitch`; the name is the vanilla key and an empty name silences it. A button
whose own line in a menu file plays a sound plays that sound alone, never the click on top of it.

Related: [Configuration](index.md), [Moments](../features/moments.md)
