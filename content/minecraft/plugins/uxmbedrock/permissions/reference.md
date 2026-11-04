---
title: Reference
order: 701
description: Every node, its default and what it grants.
icon: key-round
---

| Node | Default | Grants |
|---|---|---|
| `uxmbedrock.language` | everyone | `/bedrock lang`, `lang set`, `lang reset` |
| `uxmbedrock.settings` | everyone | `/bedrock` and its words: the settings form, menus, features, animated pack |
| `uxmbedrock.menus` | everyone | `/menus` |
| `uxmbedrock.admin.version` | op | `/uxmbedrock`, `/uxmbedrock version` |
| `uxmbedrock.admin.reload` | op | `/uxmbedrock reload` |
| `uxmbedrock.admin.doctor` | op | `/uxmbedrock doctor` |
| `uxmbedrock.admin.status` | op | `/uxmbedrock status` |
| `uxmbedrock.admin.pack` | op | `pack build`, `pack report`, `pack sync`, `items unknown`, `skulls` |
| `uxmbedrock.admin.menus` | op | `/uxmbedrock menus` |
| `uxmbedrock.admin.gui` | op | `gui measure`, `gui list`, `preview` |
| `uxmbedrock.admin.language` | op | `lang set-player`, `lang reset-player` |
| `uxmbedrock.admin.*` | op | Every `uxmbedrock.admin.` node |
| `uxmbedrock.*` | op | Every node above |

A player without `uxmbedrock.settings` still gets the polish; they only cannot change it for themselves.
