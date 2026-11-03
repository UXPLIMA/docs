---
title: Messages
order: 504
description: How a line is written, the prefixes, and every block of keys with the values each fills in.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A line you delete falls back to the default built into the plugin.

| Prefix | Draws |
|---|---|
| `<tag:'GLOW'>` | The plugin's category word |
| `<etag:'ERROR'>` | An error |
| `<h:'...'>` | A header |

| Block | Values | Said when |
|---|---|---|
| `admin.version`, `usage`, `reloaded`, `reload-failed` | `<version>`, `<ms>` | The operator commands |
| `admin.doctor.*` | `<check>`, `<said>` | `/uxmglow doctor` |
| `glow.set`, `cycling`, `cleared`, `already-off`, `restored`, `lapsed` | `<colour>`, `<colours>` | The moments and `/glow off` |
| `colours.available`, `none` | `<colours>` | `/glow colours` |
| `error.colour-unknown`, `colour-not-permitted`, `cycle-not-permitted`, `cycle-too-few`, `cycle-too-many`, `cycle-interval-too-short` | `<name>`, `<colour>`, `<limit>`, `<min>` | Refusals |
| `menu.title`, `missing`, `lore.*`, `button.*` | `<menu>` | The picker |
| `menu.colour.free`, `worn`, `locked` | `<colour_name>` | A colour's tile |
| `menu.off.on`, `off` | | The off button |
| `command.*` | | The command layer: help pages, rejected arguments |
| `language.*` | `<language>`, `<languages>`, `<player>`, `<input>` | `/glow lang` |

`<colour>` and `<colours>` are painted in the colour they name.

Related: [Languages](languages.md), [theme.conf](theme.md)
