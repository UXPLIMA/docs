---
title: Reference
order: 701
description: Every node uxmKotl declares, its default, and what it grants.
icon: list
---

Every node is declared in the plugin, so a permission plugin lists them all. Players can play on a fresh install
without any setup: `uxmkotl.play` and `uxmkotl.language` are everyone's.

## Players

| Node | Default | Grants |
|---|---|---|
| `uxmkotl.play` | everyone | `/kotl` and every subcommand under it: the arena list, `join`, `leave`, `stats`, `top`, and the two windows |
| `uxmkotl.language` | everyone | `/kotl lang`, `lang set`, `lang reset`: reading this plugin in a language of your own |

Joining by walking into an arena checks no node. To keep a group out of every arena, take `uxmkotl.play` away and
switch `join-on-enter` off on the arenas.

## Operators

| Node | Default | Grants |
|---|---|---|
| `uxmkotl.admin.*` | op | Every node below, through a permission plugin that reads wildcards, such as LuckPerms |
| `uxmkotl.admin.create` | op | `/uxmkotl create` |
| `uxmkotl.admin.edit` | op | `set` and `edit` |
| `uxmkotl.admin.enable` | op | `enable` |
| `uxmkotl.admin.disable` | op | `disable` |
| `uxmkotl.admin.delete` | op | `delete` |
| `uxmkotl.admin.list` | op | `list` and `show` |
| `uxmkotl.admin.tp` | op | `tp` |
| `uxmkotl.admin.reset` | op | `reset`: taking a player off the record and every board |
| `uxmkotl.admin.reload` | op | `reload` |
| `uxmkotl.admin.doctor` | op | `doctor` |
| `uxmkotl.admin.version` | op | `version`, and the bare `/uxmkotl`, which opens the arena list |
| `uxmkotl.admin.language` | op | `/kotl lang set-player` and `lang reset-player`: another player's language |

<Callout type="warning" title="The windows do not check the nodes again">

The buttons of the arena list and the editor act for whoever has the window open. An operator who can open the list
with `uxmkotl.admin.list` or `uxmkotl.admin.version` can create, edit, switch and delete arenas from it without
`create`, `edit`, `enable`, `disable` or `delete`. Give a moderator who should only read the list `show` through the
console instead, or give them nothing here.

</Callout>

## A working setup

| Group | Give |
|---|---|
| Everyone | nothing: the defaults already let them play |
| A builder | `uxmkotl.admin.create`, `uxmkotl.admin.edit`, `uxmkotl.admin.enable`, `uxmkotl.admin.disable`, `uxmkotl.admin.list`, `uxmkotl.admin.tp` |
| A moderator | `uxmkotl.admin.reset` |
| An administrator | `uxmkotl.admin.*` |

With LuckPerms:

```
lp group builder permission set uxmkotl.admin.edit true
lp group admin permission set uxmkotl.admin.* true
```

Related: [Player commands](../commands/player.md), [Operator commands](../commands/admin.md)
