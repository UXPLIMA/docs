---
title: Overview
order: 201
description: The two roots, renaming them, what runs from the console, and who sees what.
icon: list-tree
---

uxmDailyRewards has two commands.

| Root | Aliases | Page |
|---|---|---|
| `/dailyrewards` | `/daily`, `/rewards` | [Player commands](player.md): the calendar, the claim, the language |
| `/uxmdailyrewards` | `/uxmdr`, `/dailyrewardsadmin` | [Operator commands](admin.md) |

## Renaming

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md). The key on the left never
changes; the name on the right is yours. A subcommand set to `enabled = false` leaves the tree; a root set to
`enabled = false` is never registered. A change needs a restart: the server builds its command tree once, at start.
`localized-aliases` gives a command a word for the readers of one language, as in every UXM plugin.

## Console

`reload`, `version`, `doctor`, `reset`, `grant` and the two language commands for another player run from the
console. Opening the calendar, claiming and choosing your own language need a player.

## Permissions

A sender without a command's node does not see it. `/dailyrewards` and every subcommand under it need
`uxmdailyrewards.use`, which everyone has; the language subcommands need their own node as well. Every operator
subcommand has its own `uxmdailyrewards.admin.<action>` node, all `op` by default. See the
[permission reference](../permissions/reference.md).

## Notes

- **The bare `/dailyrewards`** opens the calendar.
- **The bare `/uxmdailyrewards`** points to `/uxmdailyrewards help`, and needs `uxmdailyrewards.admin.version`.
- **Every root has a help page.** `help` after a root lists the subcommands the sender may use.

Related: [Player commands](player.md), [Operator commands](admin.md), [commands.conf](../config/commands-conf.md)
