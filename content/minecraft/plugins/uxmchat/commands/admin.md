---
title: Admin commands
order: 304
description: Reload, version, doctor, and setting the language of another player.
icon: wrench
---

`/uxmchat`, alias `/chatadmin`. Every node is given to operators by default and is a child of `uxmchat.admin.*`.

| Command | Node | What it does |
|---|---|---|
| `/uxmchat` | `uxmchat.admin.version` | The version, the same as `version` |
| `/uxmchat reload` | `uxmchat.admin.reload` | Reads the files again and swaps them in whole; turns modules on and off |
| `/uxmchat version` | `uxmchat.admin.version` | The version this server runs |
| `/uxmchat doctor` | `uxmchat.admin.doctor` | A header and a closing line; it carries no checks of its own yet |
| `/uxmchat lang set-player <player> <language>` | `uxmchat.admin.language` | Sets the language another player reads; a player who is away reads it at their next join |
| `/uxmchat lang reset-player <player>` | `uxmchat.admin.language` | Clears it |
| `/uxmchat help` | | The commands you may run |

## What a reload reads

`config.conf`, `formats.conf`, `currencies.conf`, every module file but the custom commands, the messages and the
menus. Channels, tags and announcement bundles live in the database and are changed in game. `commands.conf`,
`custom-commands.conf`, `storage` and `network` need a restart.

Related: [Configuration](../config/index.md), [Permissions](../permissions/reference.md#admin)
