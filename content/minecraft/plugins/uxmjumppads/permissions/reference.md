---
title: Reference
order: 601
description: Every node uxmJumpPads declares, its default, what it grants, and the nodes of pad types.
icon: list
---

Using a pad needs no node unless its type names one. A node held by everybody (`default: true`) keeps its command visible, and a sender without it is refused with
`command.no-permission`; any other node hides its command from a sender without it.

## Declared nodes

| Node | Default | Grants |
|---|---|---|
| `uxmjumppads.language` | everyone | `/jumppad lang`, `lang set`, `lang reset` |
| `uxmjumppads.admin.*` | op | Every node below, as declared children: it works on any permission plugin |
| `uxmjumppads.admin.edit` | op | `set`, `remove` and `list` |
| `uxmjumppads.admin.reload` | op | `reload` |
| `uxmjumppads.admin.doctor` | op | `doctor` |
| `uxmjumppads.admin.version` | op | `version`, and the bare `/uxmjumppads` |
| `uxmjumppads.admin.language` | op | `/jumppad lang set-player` and `lang reset-player` |

## Nodes of pad types

A pad type with `permission = "<node>"` throws only players who hold that node. The node is yours to name; the plugin
does not declare it, so it defaults to nobody but operators on most permission plugins until you grant it.

| Example | Grant to |
|---|---|
| `lobby.pads.vip` | The group whose pads are faster |
| `lobby.pads.staff` | Staff, for a pad onto a roof players should not reach |

A player without the node walks over the pad as over any block, and nothing tells them why.

With LuckPerms:

```
lp group vip permission set lobby.pads.vip true
lp group builder permission set uxmjumppads.admin.edit true
```

Related: [Pad types](../pads/types.md), [Operator commands](../commands/admin.md)
