---
title: Overview
order: 301
description: The two roots, renaming them, what runs from the console, and who sees what.
icon: list-tree
---

| Root | Aliases | Page |
|---|---|---|
| `/glow` | `/glowcolor`, `/glowcolour` | [Player commands](player.md) |
| `/uxmglow` | `/uxmg`, `/glowadmin` | [Operator commands](admin.md) |

## Renaming

Both names, their aliases and every word under them are values in [`commands.conf`](../config/commands-conf.md). A
rename takes effect on the next start, because a server builds its command tree once. A command you turn off leaves
the tree rather than answering with a refusal.

## Console

Every `/glow` subcommand needs a player except `lang set-player` and `lang reset-player`. Every `/uxmglow` subcommand
runs from the console.

## Permissions

`/glow` and every subcommand under it need `uxmglow.use`, held by everyone, and each subcommand its own node beside
it. A player who lost `uxmglow.use` or `uxmglow.language` still sees the command and is told
`command.no-permission`; a sender without an operator node does not see that subcommand.

## Notes

- **The bare `/glow`** opens the [picker](../menus/picker.md).
- **The bare `/uxmglow`** says to add `help`.
- **Tab completion** offers only the colours the sender holds. An interface that offers what it then refuses is a bad
  one.

Related: [Player commands](player.md), [Operator commands](admin.md)
