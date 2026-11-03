---
title: Admin commands
order: 203
description: Reload, version, doctor, and another player's language.
icon: wrench
---

| Command | Node | What it does |
|---|---|---|
| `/uxmmultilobby` | `uxmmultilobby.admin.version` | The version |
| `/uxmmultilobby reload` | `uxmmultilobby.admin.reload` | Reads every file again and swaps them in as one snapshot; a file that does not parse leaves the previous snapshot in place |
| `/uxmmultilobby version` | `uxmmultilobby.admin.version` | The version this server runs |
| `/uxmmultilobby doctor` | `uxmmultilobby.admin.doctor` | One line per check: `windows`, `placeholders` |
| `/lobby lang set-player <player> <language>` | `uxmmultilobby.admin.language` | Sets the language another player reads |
| `/lobby lang reset-player <player>` | `uxmmultilobby.admin.language` | Clears it |

`commands.conf` needs a restart.

Related: [Permissions](../permissions/reference.md)
