---
title: Reference
order: 701
description: Every permission node of uxmLang with its default and what it allows.
icon: list-checks
---

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmlang.use` | everyone | `/lang` and its subcommands, and the window |

Take it from a server's default group to keep everybody in one language without forcing it; a player who lost it still
sees `/lang` and is told `command.no-permission`.

## Operators

| Node | Default | Allows |
|---|---|---|
| `uxmlang.admin.*` | op | Declared as the family. It lists no children: grant the nodes below, or use a permission plugin's wildcard |
| `uxmlang.admin.get` | op | `/uxmlang get` |
| `uxmlang.admin.set` | op | `/uxmlang set` and `reset` |
| `uxmlang.admin.doctor` | op | `/uxmlang doctor` |
| `uxmlang.admin.version` | op | `/uxmlang` and `/uxmlang version` |
| `uxmlang.admin.reload` | op | `/uxmlang reload` |

Related: [Commands](../commands/overview.md)
