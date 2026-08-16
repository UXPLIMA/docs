---
title: Installation
order: 900
description: Requirements, the first start, the files you get, and how to verify the install.
---

uxmEssentials needs Paper 26.1.2 or newer on Java 25. Folia is supported: every scheduled task goes through
Folia-safe schedulers. Older Bukkit and Spigot builds are not supported.

## Install

1. Download the `uxmEssentials` jar. That one jar is the whole plugin.
2. Put it in `plugins/`.
3. Start the server once. The first start creates the data folder, applies the database migrations and
   writes every config file.

```
[uxmEssentials] Enabling uxmEssentials...
[uxmEssentials] Database migrations applied
[uxmEssentials] 25 of 34 feature modules enabled
[uxmEssentials] uxmEssentials enabled!
```

Storage is SQLite by default, in `plugins/uxmEssentials/data/`. There is nothing to install and nothing to
configure. MySQL, MariaDB and PostgreSQL are for networks; see [Database](../database/sqlite.md).

## What the first start writes

```
plugins/uxmEssentials/
├── config.conf     globals: storage, locale, network, claims, links
├── modules/        one folder per module, each with its own config.conf
├── messages/       player-facing text per language, in MiniMessage
├── commands/       rename, alias or disable any command
├── menus/          your own menus
└── data/           the SQLite database
```

Files are HOCON, not YAML. An update adds its new keys and keeps your values, and a key you delete falls
back to its default. See [Core Concepts](concepts.md).

## Verify

| Command | What it tells you |
|---|---|
| `/uxmess status` | Every module and whether it is enabled |
| `/uxmess doctor` | Health checks: database, economy provider, soft-depends, threading |
| `/uxmess help` | The admin subcommands |

The admin root is `/uxmess`, with the aliases `/uxmessentials` and `/uxe`. A clean `/uxmess doctor` means the
install is done.

## Companion jars

These are optional and only matter on a network.

| Jar | Install on | What it adds |
|---|---|---|
| `uxmessentials-velocity` | The Velocity proxy | Cross-server sync of homes, warps, economy and more |
| `uxmEssentials-redis` | Backend servers | The Redis transport for the same sync |
| `uxmessentials-discord` | One backend server | The Discord bridge for notifications and `/link` |

Installing one changes nothing on its own. Cross-server sync stays off until you enable it in `config.conf`;
see [Cross-Server](../cross-server/overview.md).

## If something is missing

A command that does nothing usually belongs to a disabled module. Run `/uxmess status`, set `enabled = true`
in that module's `config.conf`, then `/uxmess reload <module>`.

A plugin that fails to load is almost always a Java version below 25. Check with `java -version`.
