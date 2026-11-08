---
title: config.yml
order: 152
description: The license, language, database, worlds, the Auto Kill mode and the debug messages.
icon: file-cog
---

`config.yml` holds the settings the whole plugin shares. The rest is in
[general-settings.yml](general-settings.md).

## License and language

```yaml
license-key: ""

config:
  version: 5.0.0
  lang: en
```

| Setting | Meaning |
|---|---|
| `license-key` | Your license key. Without a valid one the plugin does not start |
| `config.version` | Written by the plugin to update your files when a new version is installed. Do not change it |
| `config.lang` | `en`, `tr`, `pl`, `de`, `fr`, or a language you added. See [Language and Menus](languages.md) |

## Database

```yaml
database:
  type: SQLITE
  host: "localhost"
  port: 3306
  database: "database"
  username: ""
  password: ""
  use-ssl: false
  use-extra-flags: false
  flush-interval-seconds: 5
```

| Setting | Meaning |
|---|---|
| `type` | `SQLITE` keeps everything in a file in the plugin folder. `MYSQL` uses a MySQL or MariaDB server. Any other value is read as `SQLITE` |
| `database` | With SQLite, the file name: `database` is `database.db`. With MySQL, the schema, which has to exist |
| `host`, `port`, `username`, `password` | MySQL only |
| `use-ssl` | MySQL only: connect over SSL |
| `use-extra-flags` | MySQL only: two extra driver flags some hosts need. Leave it off unless the connection fails without them |
| `flush-interval-seconds` | How often what spawners produce is written. What players do is written at once |

The plugin keeps every spawner in memory and the database follows it. A crash loses at most
`flush-interval-seconds` of production, never a placed spawner or a player's change. Every server
needs a database of its own: two servers on one overwrite and delete each other's spawners.

Changing the database needs a restart. Switching from SQLite to MySQL starts on an empty database:
the plugin does not copy spawners between them. When a new version changes the tables, a SQLite file
is copied to `backups/` first; back up a MySQL database yourself before updating.

## Worlds

```yaml
world-management:
  world-list:
    - testWorld
  list-type: BLACKLIST
```

| `list-type` | Spawners can be placed and broken |
|---|---|
| `BLACKLIST` | Everywhere except the worlds in `world-list` |
| `WHITELIST` | Only in the worlds in `world-list` |

This applies to vanilla spawners too. In a disabled world, `uxmspawners.bypassplace` still places and
`uxmspawners.bypassbreak` or bypass mode still breaks, with the owner and break rules applied.
Spawners already in a disabled world keep producing and their menus still open. World names are
matched exactly, case included.

## Auto Kill mode

```yaml
use-modern-auto-kill:
  enabled: true
```

Leave it on. See [Auto Kill](../features/auto-kill.md#modern-and-legacy).

## Debug

```yaml
debug: false
debug-categories:
  - NONE
ignore: []
debug-targets: []
```

| Setting | Meaning |
|---|---|
| `debug` | Turns the debug messages on. They go to the console and to players with `uxmspawners.seedebugs` |
| `debug-categories` | Which ones: `ALL`, or any of `PRODUCTION`, `AUTO_KILL`, `STORAGE`, `SELL`, `BREAK`, `PLACE`, `STACK`, `BOOST`, `HOLOGRAM`, `REGION`, `SAVE`, `GENERAL` |
| `ignore` | Messages to leave out, by name, such as `PRODUCTION_CYCLE` |
| `debug-targets` | Only these spawners, as `world,x,y,z`, and players, by UUID. Empty is all of them |

Turn it on when UXPLIMA asks you to, for one category and one spawner at a time: `ALL` on a busy
server writes thousands of lines a minute. For a single spawner, `/usp debug` is quicker. See
[Commands](../commands.md#debug).

Related: [Configuration](index.md), [Installation](../getting-started/installation.md)
