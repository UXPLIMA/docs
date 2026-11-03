---
title: Overview
order: 201
description: The one root, renaming it, what runs from the console, and who sees what.
icon: list-tree
---

uxmTeleportBow has one command. Shooting needs no command and no permission: a player who holds the bow uses it.

| Root | Aliases | Subcommands |
|---|---|---|
| `/uxmteleportbow` | `/tpbow`, `/uxmtb` | [Player commands](player.md): `lang`, `lang set`, `lang reset`. [Operator commands](admin.md): `give`, `reload`, `version`, `doctor`, `lang set-player`, `lang reset-player` |

## Renaming

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md). The key on the left never
changes; the name on the right is yours. A subcommand set to `enabled = false` leaves the tree; the root set to
`enabled = false` is never registered. A change needs a restart: the server builds its command tree once, at start.
`localized-aliases` gives a command a word for the readers of one language, as in every UXM plugin.

## Console

`give <player>`, `reload`, `version`, `doctor` and the two language commands for another player run from the console.
`give` with no player, `lang`, `lang set` and `lang reset` are about the sender, so they need a player.

## Permissions

A sender without a subcommand's node does not see it, in the help page or in tab completion. Every operator
subcommand has its own `uxmteleportbow.admin.<action>` node, all `op` by default; the three language subcommands
need `uxmteleportbow.language`, which everyone has. See the [permission reference](../permissions/reference.md).

## Notes

- **The bare `/uxmteleportbow`** points to `/uxmteleportbow help`, and needs `uxmteleportbow.admin.version`.
- **Every root has a help page.** `help` after the root lists the subcommands the sender may use.

Related: [Player commands](player.md), [Operator commands](admin.md), [commands.conf](../config/commands-conf.md)
