---
title: Overview
order: 201
description: The command at a glance, the language branch for players, and who sees what.
icon: list
---

| Command | Aliases | Subcommands |
|---|---|---|
| `/uxmjoinitems` | `/joinitems`, `/uxmji` | `give`, `giveall`, `clear`, `reload`, `version`, `doctor`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player`, `help` |

Names and aliases are the defaults from [commands.conf](../config/commands-conf.md); a change needs a restart.

## Players

A player has no command of their own but the language branch, node `uxmjoinitems.language`, given to everyone:

| Command | What it does |
|---|---|
| `/uxmjoinitems lang` | Says which language you read this plugin in, and which ones exist |
| `/uxmjoinitems lang set <language>` | Reads this plugin in that language |
| `/uxmjoinitems lang reset` | Follows your client again |

A player whose rank had the node taken away still sees the branch and is told they may not use it. The admin nodes are
operators only and hide their subcommands from everybody else.

Related: [Admin commands](admin.md), [Permissions](../permissions/reference.md)
