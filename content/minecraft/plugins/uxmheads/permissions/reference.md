---
title: Reference
order: 601
description: Every permission node of uxmHeads with its default.
icon: list-checks
---

| Permission | What it allows | Default |
|---|---|---|
| `uxmheads.language` | `/uxmheads lang`, `lang set`, `lang reset` | everyone |
| `uxmheads.admin.*` | Declared as the family; it lists no children, so grant the nodes below | op |
| `uxmheads.admin.preview` | See any player's face as it is drawn in chat | op |
| `uxmheads.admin.refresh` | Read a player's skin again | op |
| `uxmheads.admin.welcome` | Send a player the welcome again | op |
| `uxmheads.admin.doctor` | Ask how this plugin is | op |
| `uxmheads.admin.version` | `/uxmheads`, `/uxmheads version` | op |
| `uxmheads.admin.reload` | Read the configuration again | op |
| `uxmheads.admin.language` | Set or clear the language another player reads | op |

Seeing faces needs no node: every player receives the welcome and every placeholder answers for everyone.

Related: [Operator commands](../commands/admin.md)
