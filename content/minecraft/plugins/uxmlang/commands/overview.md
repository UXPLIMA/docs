---
title: Overview
order: 201
description: Both commands at a glance, renaming them, and who sees what.
icon: list
---

| Command | Aliases | Subcommands | Node |
|---|---|---|---|
| `/lang` | `/language`, `/dil` | `list`, `set`, `reset` | `uxmlang.use` |
| `/uxmlang` | `/langadmin` | `reload`, `version`, `get`, `set`, `reset`, `doctor`, `help` | `uxmlang.admin.*` nodes |

Names and aliases are the defaults from [commands.conf](../config/commands-conf.md), which renames, aliases or turns
off each command and each subcommand. A server in one language renames `/lang` to the word its players know.

`uxmlang.use` is given to everyone, so a player whose rank had it taken away still sees `/lang` and is told they may
not use it. The `uxmlang.admin` nodes are operators only and hide their subcommands from everybody else.

Related: [Player commands](player.md), [Admin commands](admin.md)
