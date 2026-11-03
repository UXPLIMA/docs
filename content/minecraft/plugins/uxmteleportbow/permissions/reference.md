---
title: Reference
order: 501
description: Every node uxmTeleportBow declares, its default, and what it grants.
icon: list
---

Using the bow needs no node: a player who holds it shoots it. What the server decides is who is given one, through
`item.give` and the world filter. A node on a command hides it: a sender without it does not see the command.

| Node | Default | Grants |
|---|---|---|
| `uxmteleportbow.language` | everyone | `lang`, `lang set`, `lang reset` |
| `uxmteleportbow.admin.give` | op | `give`, to yourself or another player |
| `uxmteleportbow.admin.reload` | op | `reload` |
| `uxmteleportbow.admin.doctor` | op | `doctor` |
| `uxmteleportbow.admin.version` | op | `version`, and the bare `/uxmteleportbow` |
| `uxmteleportbow.admin.language` | op | `lang set-player` and `lang reset-player` |
| `uxmteleportbow.admin.*` | op | Every `admin` node, on a permission plugin that reads wildcards |

`uxmteleportbow.admin.*` declares no children, so it grants the nodes under it only through a permission plugin that
reads a `*` as a wildcard, as LuckPerms does. On one that does not, grant each node.

With LuckPerms:

```
lp group helper permission set uxmteleportbow.admin.give true
lp group admin permission set uxmteleportbow.admin.* true
```

## Keeping the bow from a group

There is no node for shooting. To keep the bow from some players, do not hand it to them: turn off the `item.give`
switches and give it with a command that names who gets it, such as a `[console] uxmteleportbow give %player_name%`
line in another plugin's join actions, guarded by its own permission.

Related: [Operator commands](../commands/admin.md), [The bow and the arrow](../bow/the-bow.md)
