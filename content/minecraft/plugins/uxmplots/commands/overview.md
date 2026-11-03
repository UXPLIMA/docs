---
title: Overview
order: 201
description: The three commands at a glance, renaming them, and who sees what.
icon: list
---

| Command | Aliases | Subcommands |
|---|---|---|
| `/plot` | `/plots` | Ninety words; see [Plot commands](plot.md) |
| `/contest` | `/contests` | `info`, `enter` (`join-with`), `join`, `invite`, `leave` (`withdraw`), `vote` (`judge`), `tour`, `scores`, `entries`, `hall` (`fame`), `edit` |
| `/uxmplots` | `/plotadmin` | `reload`, `version`, `doctor` (`health`), `reports`, `report`, `inactive`, `staff`, `pregen stop`, `pregen start`, `world list`, `world`, `pin`, `unpin`, `reset`, `snapshot`, `owner`, `delete`, `unlock`, `redstone`, `phase`, `board plant`, `board uproot` |

Names and aliases are the defaults from [commands.conf](../config/commands-conf.md); a change needs a restart. A word
under a word, such as `rate part`, is written with a space.

## Who sees what

Every player word carries its own node, and every player node is given to everyone by default, so a player whose rank
had one taken away still sees the word and is told they may not use it. `uxmplots.contest.edit`,
`uxmplots.template.save` and every `uxmplots.admin` node are operators only and hide their words from everybody else.

## Standing on a plot

Most `/plot` words act on the plot you stand on: `claim`, `info`, `rate`, `trust`, `flag`, `reset` and the rest.

Related: [Plot commands](plot.md), [Permissions](../permissions/reference.md)
