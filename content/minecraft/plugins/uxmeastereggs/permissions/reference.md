---
title: Reference
order: 701
description: Every node uxmEasterEggs declares, its default, and what it grants.
icon: list
---

Every node is declared in the plugin, so a permission plugin lists them all. Players can hunt on a fresh install
without any setup, and finding an egg needs no node at all.

A node held by everybody (`default: true`) keeps its command visible, and a sender without it is refused with
`command.no-permission`; any other node hides its command from a sender without it.

## Players

| Node | Default | Grants |
|---|---|---|
| `uxmeastereggs.use` | everyone | `/eggs`: the collection, `radar` and `top` |
| `uxmeastereggs.language` | everyone | `/eggs lang`, `lang set`, `lang reset` |

## Operators

| Node | Default | Grants |
|---|---|---|
| `uxmeastereggs.admin` | op | The whole `/uxmeastereggs` root, its bare form, `version`, and every node below as children |
| `uxmeastereggs.admin.create` | op | `create`, `set`, and the type picker |
| `uxmeastereggs.admin.delete` | op | `delete` |
| `uxmeastereggs.admin.list` | op | `list` and `show` |
| `uxmeastereggs.admin.editor` | op | `editor`, the placed eggs window |
| `uxmeastereggs.admin.tp` | op | `tp` |
| `uxmeastereggs.admin.reset` | op | `reset`: taking every find of a player away and the player off every board |
| `uxmeastereggs.admin.reload` | op | `reload` |
| `uxmeastereggs.admin.doctor` | op | `doctor` |
| `uxmeastereggs.admin.language` | op | `/eggs lang set-player` and `lang reset-player` |

<Callout type="warning" title="A child node alone shows nothing">

`uxmeastereggs.admin` guards the whole operator root, so a builder given only `uxmeastereggs.admin.create` cannot see
`/uxmeastereggs` at all. Give `uxmeastereggs.admin` and deny the children they should not have.

</Callout>

`uxmeastereggs.admin` declares the eight action nodes as children, so it grants them on any permission plugin,
wildcards or not. It does not grant `uxmeastereggs.admin.language`.

## A working setup

| Group | Give |
|---|---|
| Everyone | nothing: the defaults already let them hunt |
| A builder | `uxmeastereggs.admin`, then deny `uxmeastereggs.admin.reset`, `uxmeastereggs.admin.reload` and `uxmeastereggs.admin.delete` |
| An administrator | `uxmeastereggs.admin` |

With LuckPerms:

```
lp group builder permission set uxmeastereggs.admin true
lp group builder permission set uxmeastereggs.admin.reset false
lp group builder permission set uxmeastereggs.admin.delete false
```

A builder denied `admin.delete` can still remove an egg with a shift click in the placed eggs window, which checks no
node of its own; deny `uxmeastereggs.admin.editor` too if that matters.

Related: [Player commands](../commands/player.md), [Operator commands](../commands/admin.md)
