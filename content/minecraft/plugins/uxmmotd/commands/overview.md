---
title: Overview
order: 201
description: The command at a glance, renaming it, and who may run it on each platform.
icon: list
---

| Command | Aliases | Subcommands |
|---|---|---|
| `/uxmmotd` | `/motd` | `reload`, `version`, `maintenance`, `maintenance on`, `maintenance off`, `maintenance status`, `doctor`, `help` |

Players have no command: everything a player sees is in the server list. The name, aliases and switch of the command
and of each word are in [commands.conf](../config/commands-conf.md); a change needs a restart.

On Paper the nodes are operators only by default and hide their subcommands from everybody else. On Velocity there are
no defaults: the console may run everything and a player needs the node from a permission plugin.

Related: [Admin commands](admin.md), [Permissions](../permissions/reference.md)
