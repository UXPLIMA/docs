---
title: Operator commands
order: 203
description: Every /uxmhubpvp subcommand, with its arguments, what it does and the permission it needs.
icon: shield
---

Every operator command is under `/uxmhubpvp`, aliases `/hubpvp` and `/pvpadmin`. Each has its own node, all `op` by
default, and every one runs from the console.

| Command | Permission | What it does |
|---|---|---|
| `/uxmhubpvp` | `uxmhubpvp.admin.version` | Points to `/uxmhubpvp help` |
| `/uxmhubpvp reset <player>` | `uxmhubpvp.admin.reset` | Takes a player off the record and off every board, in every period, by the name they last played under. The rows go on every server sharing the database |
| `/uxmhubpvp reload` | `uxmhubpvp.admin.reload` | Reads `config.conf`, `kit.conf`, the messages and the theme again. See [Configuration](../config/index.md#reloading) |
| `/uxmhubpvp doctor` | `uxmhubpvp.admin.doctor` | Checks the storage and PlaceholderAPI, one line each. See [Installation](../getting-started/installation.md#checking-the-install) |
| `/uxmhubpvp version` | `uxmhubpvp.admin.version` | Prints the version this server runs |

`reset` answers `stats.unknown` for a name that never played here, and `stats.reset` when the rows are gone. It runs
off the main thread and answers when the database is done.

Related: [Overview](overview.md), [Permissions](../permissions/reference.md)
