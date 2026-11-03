---
title: Overview
order: 201
description: Both commands at a glance, renaming them, and who sees what.
icon: list
---

| Command | Aliases | Subcommands |
|---|---|---|
| `/minions` | `/minion`, `/mn` | `list` (`l`), `report`, `trust`, `untrust`, `name`, `consume`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmminions` | `/minionsadmin` | `give`, `find`, `open`, `minions`, `remove`, `restore`, `slots`, `ledger`, `migrate`, `editor`, `reload`, `doctor`, `version` |

Names and aliases are the defaults from [commands.conf](../config/commands-conf.md); a change needs a restart.

## Who sees what

`/minions` needs no node: every player has it. Every `/uxmminions` subcommand hides behind its `uxmminions.admin` node,
and so do `lang set-player` and `lang reset-player`.

## Ids

A minion is named by its stable id everywhere: on its panel, in `/uxmminions find`, and in the placeholders. A
coordinate is where a minion is, never who it is.

Related: [Player commands](player.md), [Admin commands](admin.md)
