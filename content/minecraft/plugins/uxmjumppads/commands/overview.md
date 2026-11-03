---
title: Overview
order: 201
description: The two roots, renaming them, and what runs from the console.
icon: list-tree
---

uxmJumpPads has two commands. A pad needs no command: walking onto it is the whole of using it.

| Root | Aliases | Page |
|---|---|---|
| `/jumppad` | `/jumppads` | [Player commands](player.md): the language a player reads |
| `/uxmjumppads` | `/uxmjp`, `/jumppadadmin` | [Operator commands](admin.md) |

## Renaming

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md). The key on the left never
changes; the name on the right is yours. A subcommand set to `enabled = false` leaves the tree; a root set to
`enabled = false` is never registered. A change needs a restart: the server builds its command tree once, at start.
`localized-aliases` gives a command a word for the readers of one language, as in every UXM plugin.

## Console

`list`, `reload`, `version`, `doctor` and the two language commands for another player run from the console. `set`
and `remove` read the block a player stands on, so they need a player.

## Permissions

A sender without a command's node does not see it. Every operator subcommand has its own `uxmjumppads.admin.<action>`
node, all `op` by default. See the [permission reference](../permissions/reference.md).

## Notes

- **The bare `/uxmjumppads`** points to `/uxmjumppads help`, and needs `uxmjumppads.admin.version`.
- **Every root has a help page.** `help` after a root lists the subcommands the sender may use.

Related: [Player commands](player.md), [Operator commands](admin.md), [commands.conf](../config/commands-conf.md)
