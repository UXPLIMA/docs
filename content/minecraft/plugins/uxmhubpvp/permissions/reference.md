---
title: Reference
order: 501
description: Every node uxmHubPvP declares, its default, and what it grants.
icon: list
---

Every node is declared in the plugin, so a permission plugin lists them all. Players can fight on a fresh install
without any setup. A node on a command hides it: a player without it does not see the command in tab completion,
and the server answers as if it did not exist.

## Players

| Node | Default | Grants |
|---|---|---|
| `uxmhubpvp.use` | everyone | `/pvp` and every branch under it; the bare `/pvp` presses the toggle |
| `uxmhubpvp.stats` | everyone | `/pvp stats`, your own record and another player's |
| `uxmhubpvp.top` | everyone | `/pvp top` |
| `uxmhubpvp.language` | everyone | `/pvp lang`, `lang set`, `lang reset`: reading this plugin in a language of your own |
| `uxmhubpvp.bypass.commands` | op | Typing any command while combat tagged |

`stats` and `top` need `uxmhubpvp.use` as well, because it guards the whole `/pvp` command. The toggle item checks no
node: a player who holds it can press it.

## Operators

| Node | Default | Grants |
|---|---|---|
| `uxmhubpvp.admin.*` | op | Every operator node, through a permission plugin that reads wildcards, such as LuckPerms |
| `uxmhubpvp.admin.reset` | op | `reset`: taking a player off the record and every board |
| `uxmhubpvp.admin.reload` | op | `reload` |
| `uxmhubpvp.admin.doctor` | op | `doctor` |
| `uxmhubpvp.admin.version` | op | `version`, and the bare `/uxmhubpvp` |
| `uxmhubpvp.admin.language` | op | `/pvp lang set-player` and `lang reset-player`: another player's language |

<Callout type="warning" title="Operators type anything in a fight">

`uxmhubpvp.bypass.commands` is an operator's by default, so an operator who fights can teleport out of it. Test the
combat tag with a player who is not an operator, and deny the node to staff who fight for real.

</Callout>

## A working setup

| Group | Give |
|---|---|
| Everyone | nothing: the defaults already let them fight |
| A moderator | `uxmhubpvp.admin.reset` |
| An administrator | `uxmhubpvp.admin.*` |
| A group kept out of fights | deny `uxmhubpvp.use`, and keep the toggle item from them |

With LuckPerms:

```
lp group default permission set uxmhubpvp.bypass.commands false
lp group admin permission set uxmhubpvp.admin.* true
```

Related: [Player commands](../commands/player.md), [Operator commands](../commands/admin.md)
