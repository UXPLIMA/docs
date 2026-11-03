---
title: Reference
order: 501
description: Every node uxmPlayerHider declares, its default, and what it grants.
icon: list
---

A node on a command hides it: a sender without it does not see the command.

## Players

| Node | Default | Grants |
|---|---|---|
| `uxmplayerhider.use` | everyone | `/hide` and everything under it. The tool works without it |
| `uxmplayerhider.language` | everyone | `/hide lang`, `lang set`, `lang reset` |
| `uxmplayerhider.important` | op | Staying visible to a viewer in the `important` state |
| `uxmplayerhider.cooldown.bypass` | op | Changing state without waiting out `cooldown.ms` |

## Operators

| Node | Default | Grants |
|---|---|---|
| `uxmplayerhider.admin.reload` | op | `reload` |
| `uxmplayerhider.admin.doctor` | op | `doctor` |
| `uxmplayerhider.admin.version` | op | `version`, and the bare `/uxmplayerhider` |
| `uxmplayerhider.admin.language` | op | `/hide lang set-player` and `lang reset-player`, with `uxmplayerhider.use` |
| `uxmplayerhider.admin.*` | op | Every `admin` node, on a permission plugin that reads wildcards |

`uxmplayerhider.admin.*` declares no children, so it grants the nodes under it only through a permission plugin that
reads a `*` as a wildcard, as LuckPerms does. On one that does not, grant each node.

## Notes

- **Taking `uxmplayerhider.use` away** hides `/hide` but leaves the tool working. To take the whole plugin from a
  group, turn the tool off as well, or keep the group out of the listed `worlds`.
- **Important is not op-only by design.** On a fresh install every operator is important; grant
  `uxmplayerhider.important` to the staff and players you want to stay visible, and take it from an operator who
  should vanish with the crowd.

With LuckPerms:

```
lp group staff permission set uxmplayerhider.important true
lp group vip permission set uxmplayerhider.cooldown.bypass true
```

Related: [States](../hiding/states.md), [Player commands](../commands/player.md)
