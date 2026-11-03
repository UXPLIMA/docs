---
title: Admin commands
order: 303
description: Reading and setting another player's language, reload, version and doctor.
icon: wrench
---

`/uxmlang`, alias `/langadmin`.

| Command | Node | What it does |
|---|---|---|
| `/uxmlang` | `uxmlang.admin.version` | The version |
| `/uxmlang reload` | `uxmlang.admin.reload` | Reads `config.conf`, `languages.conf`, the window and the language files again and swaps them in whole |
| `/uxmlang version` | `uxmlang.admin.version` | The version this server runs |
| `/uxmlang get <player>` | `uxmlang.admin.get` | Which language a player reads, whether they chose it or their client did |
| `/uxmlang set <player> <language>` | `uxmlang.admin.set` | Chooses for a player, the way they would choose for themselves |
| `/uxmlang reset <player>` | `uxmlang.admin.set` | Drops a player's choice |
| `/uxmlang doctor` | `uxmlang.admin.doctor` | One line per check: `storage`, `content`, `windows` |
| `/uxmlang help` | | The subcommands you may run |

`get`, `set` and `reset` take a player who is on this server. Resolving an offline name can reach the internet, which
does not belong on the thread that runs a command; to change an offline player's language, edit their row.

`storage`, `network` and `commands.conf` need a restart.

Related: [Permissions](../permissions/reference.md)
