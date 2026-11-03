---
title: Reference
order: 601
description: Every node uxmParkour declares, its default, and what it grants.
icon: list
---

Every node is declared in the plugin, so a permission plugin lists them all. Players can run on a fresh install
without any setup: `uxmparkour.play` and `uxmparkour.language` are everyone's.

## Players

| Node | Default | Grants |
|---|---|---|
| `uxmparkour.play` | everyone | `/parkour`, `join`, `leave`, `restart`, `themes`, `theme`, `difficulty`, `stats`, `top`, and the theme picker |
| `uxmparkour.language` | everyone | `/parkour lang`, `lang set`, `lang reset`: reading this plugin in a language of your own |

Take `uxmparkour.play` away from a group to keep it out of the parkour.

## Operators

| Node | Default | Grants |
|---|---|---|
| `uxmparkour.admin.*` | op | Every node below, through a permission plugin that reads wildcards, such as LuckPerms |
| `uxmparkour.admin.create` | op | `/uxmparkour create`, and **New arena** in the list |
| `uxmparkour.admin.edit` | op | `set`, `edit`, `enable`, `disable`, and the editor |
| `uxmparkour.admin.delete` | op | `delete` |
| `uxmparkour.admin.list` | op | `list`, `show`, and the bare `/uxmparkour` |
| `uxmparkour.admin.tp` | op | `tp` |
| `uxmparkour.admin.reset` | op | `reset`: taking a runner off the record and every board |
| `uxmparkour.admin.reload` | op | `reload` |
| `uxmparkour.admin.doctor` | op | `doctor` |
| `uxmparkour.admin.version` | op | `version` |
| `uxmparkour.admin.build` | op | Breaking and placing blocks inside an arena's box while a run is in it |
| `uxmparkour.admin.language` | op | `/parkour lang set-player` and `lang reset-player`: another player's language |

## A working setup

| Group | Give |
|---|---|
| Everyone | nothing: the defaults already let them run |
| A builder | `uxmparkour.admin.create`, `uxmparkour.admin.edit`, `uxmparkour.admin.list`, `uxmparkour.admin.tp`, `uxmparkour.admin.build` |
| A moderator | `uxmparkour.admin.reset`, `uxmparkour.admin.list` |
| An administrator | `uxmparkour.admin.*` |

With LuckPerms:

```
lp group builder permission set uxmparkour.admin.edit true
lp group admin permission set uxmparkour.admin.* true
```

Related: [Player commands](../commands/player.md), [Operator commands](../commands/admin.md)
