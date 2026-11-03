---
title: Reference
order: 601
description: Every permission node of uxmJoinItems with its default and what it allows.
icon: list-checks
---

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmjoinitems.language` | everyone | `/uxmjoinitems lang`, `lang set`, `lang reset` |

Every player gets the items whatever their nodes: who gets which item is decided by triggers, regions and guards. A
click's `[player]` and `[menu]` lines run as the player, so the commands they run check the player's own nodes.

## Operators

| Node | Default | Allows |
|---|---|---|
| `uxmjoinitems.admin.*` | op | Declared as the family. It lists no children: grant the nodes below, or use a permission plugin's wildcard |
| `uxmjoinitems.admin.give` | op | `give` |
| `uxmjoinitems.admin.giveall` | op | `giveall`. The node's description says "every player online", but the command gives one player every item |
| `uxmjoinitems.admin.clear` | op | `clear` |
| `uxmjoinitems.admin.reload` | op | `reload` |
| `uxmjoinitems.admin.version` | op | `/uxmjoinitems` and `version` |
| `uxmjoinitems.admin.doctor` | op | `doctor` |
| `uxmjoinitems.admin.language` | op | `lang set-player` and `lang reset-player` |

Related: [Commands](../commands/overview.md)
