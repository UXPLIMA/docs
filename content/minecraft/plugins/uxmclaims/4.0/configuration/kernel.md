---
title: Kernel config
order: 141
description: Every key of the root config.yml, the settings that belong to no module.
icon: file-cog
---

The root `plugins/uxmClaims/config.yml` holds what no module owns: language, storage, license, a few protection
switches and the 3.x migration. Each module's settings are in `modules/<id>/config.yml`. Durations are written
as `500ms`, `30s`, `10m`, `6h`, `30d` or `1w`; a bare number counts as seconds.

`/uxm config check` (permission `uxmclaims.admin`) lists the warnings and errors the last load of every config
and language file found, and shows the license key masked after its first four characters.

## Settings

### language

| Key | Default | What it does |
|---|---|---|
| `language.default` | `en` | Server language: `en`, `tr`, `de`, `es`, `fr`, `pl`, `pt-BR` or `ru`. [restart] |
| `language.per-player` | `false` | Each player gets messages and menus in their client language when it is in `enabled`, else the default. Console, logs, webhooks and maps keep the default. [restart] |
| `language.enabled` | `[en]` | Languages loaded besides the default when `per-player` is on; their folders are created |
| `language.locale-map` | `pt_br: pt-BR`, `pt_pt: pt-BR` | Client locale to language code. When nothing matches, the first two letters are tried |

### storage

| Key | Default | What it does |
|---|---|---|
| `storage.type` | `SQLITE` | `SQLITE`, `MYSQL`, `MARIADB` or `POSTGRESQL`. [restart] |
| `storage.sqlite.file` | `data/storage.db` | SQLite file, relative to the plugin folder |
| `storage.remote.host` | `localhost` | Database host |
| `storage.remote.port` | `0` | Database port. `0` uses the type's default: 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `storage.remote.database` | `uxmclaims` | Database name |
| `storage.remote.schema` | `public` | PostgreSQL only: the schema the tables are in. [restart] |
| `storage.remote.username` | `root` | User |
| `storage.remote.password` | `""` | Password, never logged |
| `storage.remote.properties` | `useSSL: false` | JDBC URL parameters |
| `storage.remote.jdbc-url-override` | `""` | When set, this URL is used instead of host, port and database. [restart] |
| `storage.table-prefix` | `uxmc_` | Table name prefix. Do not change it after install: the plugin would no longer see its tables. [restart] |
| `storage.pool.maximum-size` | `10` | Most open connections. [restart] |
| `storage.pool.minimum-idle` | `2` | Connections kept open while idle. [restart] |
| `storage.pool.connection-timeout` | `30s` | How long to wait for a connection. [restart] |
| `storage.pool.max-lifetime` | `30m` | How long one connection is reused. [restart] |

See [Database](database.md) for each database type.

### license, protection, api, menus

| Key | Default | What it does |
|---|---|---|
| `license.key` | `""` | Your license key. The plugin does not start without one. Never logged, masked in `/uxm config check`. [restart] |
| `protection.access-guard.enabled` | `true` | Checks where every player is and pushes them out of lands they may not be in (banned, or without `lands:move_inside`), however they got there. [restart] |
| `protection.access-guard.interval-ticks` | `20` | How often each player is checked, in ticks (20 = one second). [restart] |
| `protection.decision-cache` | `true` | Remembers role answers per player and place until something changes. Turn off only to rule it out when a permission answer looks wrong |
| `api.bukkit-events.enabled` | `true` | Mirrors uxmClaims events as Bukkit events, for Skript and plain Bukkit listeners |
| `api.bukkit-events.pre-events` | `true` | Also mirrors the cancellable `Pre*` events |
| `api.bukkit-events.movement` | `true` | Fires `LandEnterEvent` and `LandLeaveEvent` on border crossings |
| `menus.lore-width` | `40` | Lore lines longer than this many characters wrap in chest menus, keeping their colours. `0` turns it off. Lines with their own `<newline>` or `<br>` never wrap |

### config, migration

| Key | Default | What it does |
|---|---|---|
| `config.auto-fill-missing-keys` | `true` | Adds missing keys from the defaults, with their comments (at start-up) |
| `config.warn-unknown-keys` | `true` | Warns about keys the plugin does not know; they are never deleted |
| `config.keep-backups` | `20` | Copies kept under `backups/config/` of every file the plugin rewrites |
| `config.update-unchanged-defaults` | `true` | When an update ships a new text or default, uses it for every value you never changed; edited values are kept and logged once (at start-up). See [Languages](languages.md) |
| `migration.batch-size` | `1000` | Rows read and written per transaction when the 3.x data is copied (50 to 50,000) |
| `migration.sync-threshold-seconds` | `20` | Above this estimated copy time the 3.x data is copied in the background in maintenance mode (0 to 3600). See [From 3.x](../upgrading/from-3x.md) |

## Notes

- **Removed keys:** the file is at version 2. The first start of 4.0.0 deletes `updates`, `metrics`,
  `performance`, `debug` and `protection.admin-bypass` from an older file; nothing read them.
- **Admin bypass:** set `protection.admin-bypass` in `modules/lands/config.yml`. `IMPLICIT` lets OP,
  `*` and `uxmclaims.admin` bypass always (the 3.x way, written by the migration). `TOGGLE` asks for
  `uxmclaims.admin.bypass` and `/uxm bypass [on|off]`, which lasts until the admin turns it off or leaves.
- **Changing the language:** `language.default` is read at start-up; restart after changing it. The folders of
  the new language are created then; the old ones stay.
- **Migration keys** are read straight from the file by the importer, on the start that runs the 3.x move.
- After fixing a file, run `/uxm reload`; keys marked [restart] need a restart.

Related: [Configuration](index.md), [Database](database.md), [Languages](languages.md)
