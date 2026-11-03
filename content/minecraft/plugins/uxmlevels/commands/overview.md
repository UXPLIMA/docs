---
title: Overview
order: 201
description: The two roots, renaming them, what runs from the console, and who sees what.
icon: list-tree
---

uxmLevels has two commands.

| Root | Aliases | Page |
|---|---|---|
| `/levels` | `/lvl`, `/level` | [Player commands](player.md): the windows, your level, the boards, prestige, the language |
| `/uxmlevels` | `/levelsadmin` | [Operator commands](admin.md) |

## Renaming

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md). The key on the left never
changes; the name on the right is yours. A subcommand set to `enabled = false` leaves the tree; a root set to
`enabled = false` is never registered. A change needs a restart: the server builds its command tree once, at start.
`localized-aliases` gives a command a word for the readers of one language, as in every UXM plugin.

## Console

`/levels level of`, `/levels top`, the two language commands for another player, and every `/uxmlevels` subcommand
run from the console. The windows, your own level, prestige and your own language need a player.

## A player

Commands that name a player take the name of anyone online or anyone the server remembers, so an operator can give
experience to a player who is away. A name the server has never seen answers `error.unknown-player`.

## Permissions

A node held by everybody (`default: true`) keeps its command visible, and a sender without it is refused with
`command.no-permission`; any other node hides its command from a sender without it.
Each `/levels` subcommand carries its own node; each operator
subcommand its own `uxmlevels.admin.<action>` node, all `op` by default. See the
[permission reference](../permissions/reference.md).

## Notes

- **The bare `/levels`** opens the [levels window](../menus/main.md).
- **The bare `/uxmlevels`** points to `/uxmlevels help`, and needs `uxmlevels.admin.version`.
- **Every root has a help page.** `help` after a root lists the subcommands the sender may use.

Related: [Player commands](player.md), [Operator commands](admin.md), [commands.conf](../config/commands-conf.md)
