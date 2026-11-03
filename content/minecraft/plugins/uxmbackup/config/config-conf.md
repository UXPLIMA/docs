---
title: config.conf
order: 601
description: Every block and key of config.conf, with its default and what it does.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `language.default` | `"en"` | What a player reads when nothing else answers |
| `language.follow-client` | `true` | A player who chose nothing reads their client's language |
| `language.force` | `""` | One language for everybody |

## effects

`started`, `done`, `partial`, `failed`, `refused`: lists of action lines, all empty. See [Alerts](../features/alerts.md).

## storage

The run history, not the archives.

| Key | Default | Meaning |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql` |
| `file` | `"backup.db"` | The SQLite file, inside `data/` |
| `host`, `port`, `database`, `username`, `password` | `localhost`, `0`, `minecraft`, `minecraft`, `""` | A database server |
| `table-prefix` | `"uxmbackup_"` | In front of every table |
| `pool-size` | `4` | Connections; SQLite uses one |
| `local-only` | `false` | This server keeps its history in its own SQLite file |
| `copy-from-file` | `false` | Copies the SQLite rows into a new database's empty tables once |

## run

| Key | Default | Meaning |
|---|---|---|
| `work-folder` | `"work"` | Where an archive is built before it is handed to a storage; the only place the plugin deletes |
| `compression-level` | `5` | 0 stores, 9 packs hardest |
| `free-space-floor-mb` | `2048` | A run is refused when it would leave less free disk than this |
| `progress.every-seconds` | `15` | How often the console reports a running backup |
| `participants.timeout-seconds` | `30` | How long other plugins get to write their state |
| `databases.allow-external-commands` | `false` | Lets a database source name `mysqldump` or `pg_dump` as its `command` |
| `databases.external-timeout-seconds` | `600` | How long such a command may run |
| `excludes` | `libraries`, `cache`, `logs`, `crash-reports`, `spark`, `**/*.lock` | What no set takes |

## sets

One block per set: `worlds`, `paths`, `storages`, `schedule`, `excludes`, `databases`. Ships `full` and `light`. See
[Sets and schedules](../features/sets-and-schedules.md).

## storages

One block per storage: `type`, `enabled`, the type's keys, and `keep-count`, `keep-size-mb`, `keep-days`. Ships `local`
(`archives`, keep 10, 30 days) and commented examples of `sftp`, `s3`, `webdav` and `ftps`. See
[Storages](../features/storages.md).

## restore

| Key | Default | Meaning |
|---|---|---|
| `passphrase-hash` | `""` | The line `/uxmbackup hash` prints. Empty: an in-place restore is refused |
| `token-minutes` | `10` | How long the token of the first step is good for |
| `database-folder` | `"uxmbackup-restore"` | Where a dump from an archive is written, under the server folder; never applied |

## shutdown

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `false` | Run a set as the server stops |
| `set` | `"light"` | Which |
| `limit-seconds` | `120` | The most it may take |

## discord

| Key | Default | Meaning |
|---|---|---|
| `webhook-url` | `""` | A Discord webhook |
| `on-success` | `false` | Post on success |
| `on-failure` | `true` | Post on failure |

## menu

`menu.sounds.click`, `page` and `denied`, each `{ name, volume, pitch }`; an empty name silences one.

Related: [Configuration](index.md)
