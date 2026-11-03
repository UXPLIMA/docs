---
title: Overview
order: 201
description: The two roots, renaming them, what runs from the console, and who sees what.
icon: list-tree
---

uxmPlayerHider has two commands. The tool does what `/hide` does, so a player with the tool needs no command.

| Root | Aliases | Page |
|---|---|---|
| `/hide` | `/playerhider`, `/ph` | [Player commands](player.md): the state, the status, the language |
| `/uxmplayerhider` | `/uxmph`, `/hideadmin` | [Operator commands](admin.md) |

## Renaming

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md). The key on the left never
changes; the name on the right is yours. A subcommand set to `enabled = false` leaves the tree; a root set to
`enabled = false` is never registered. A change needs a restart: the server builds its command tree once, at start.
`localized-aliases` gives a command a word for the readers of one language, as in every UXM plugin.

## Console

`reload`, `version`, `doctor` and the two language commands for another player run from the console. Everything
else under `/hide` is about the sender's own view, so it needs a player.

## Permissions

A node held by everybody (`default: true`) keeps its command visible, and a sender without it is refused with
`command.no-permission`; any other node hides its command from a sender without it.
`/hide` and every subcommand under it need `uxmplayerhider.use`,
which everyone has; the language subcommands need their own node as well. Every operator subcommand has its own
`uxmplayerhider.admin.<action>` node, all `op` by default. See the [permission reference](../permissions/reference.md).

## Notes

- **The bare `/hide`** moves to the next state, as a right click on the tool does.
- **The bare `/uxmplayerhider`** points to `/uxmplayerhider help`, and needs `uxmplayerhider.admin.version`.
- **Every root has a help page.** `help` after a root lists the subcommands the sender may use.

Related: [Player commands](player.md), [Operator commands](admin.md), [commands.conf](../config/commands-conf.md)
