---
title: config.yml
order: 51
description: License, language, database, network, number format and debug.
icon: file-cog
---

`config.yml` holds what belongs to this one server. On a network, every server has its own.

## Basics

```yaml
config-version: "1.1.0"   # written by the plugin, do not change
language: en
license-key: ""
```

| Key | What it does |
|---|---|
| `language` | `en`, `tr`, `es`, `pt_br`, `de`, `fr`, `ru` or `pl`. See [Languages and menus](languages.md) |
| `license-key` | The key from your UXPLIMA purchase. Without a valid key the plugin turns itself off |

## Database

```yaml
database:
  driver: "SQLITE"
  host: "localhost"
  database: "supplyrush_database"
  username: ""
  password: ""
  port: 3306
  use-ssl: false
  use-extra-flags: false
```

`SQLITE` keeps everything in a file in the plugin folder. `MYSQL` works for MySQL and MariaDB. See
[Database](database.md).

## Network

```yaml
multi-server:
  enabled: false
  redis-host: ""
  network: "default"
  server-name: "server-1"
  stream-retention-seconds: 300
  stream-max-length: 100000
```

Leave `enabled: false` on a single server. Every key is explained on
[Multiple servers](../features/multi-server.md).

## How numbers look

```yaml
number-format:
  grouping-separator: "."
  decimal-separator: ","
```

These decide how points and amounts appear in menus, messages and placeholders: `2.440` and
`1.250,5` with the defaults, `2,440` and `1,250.5` with the two swapped. Each is one character;
`grouping-separator: ""` shows `2440`. Exported summary files always use plain numbers.

## Debug

```yaml
debug: false
debug-categories:
  - NONE
ignore: []
target-ids: []
```

Leave `debug: false` unless UXPLIMA support asks you to turn it on. Players with
`uxmsupplyrush.seedebugs` then see the debug output in chat. The categories are `NONE`, `ALL`,
`USER`, `DEPOSIT`, `EVENT`, `REWARD`, `SCHEDULE`, `CHAT` and `GENERAL`.

Related: [general-settings.yml](general-settings.md), [Database](database.md)
