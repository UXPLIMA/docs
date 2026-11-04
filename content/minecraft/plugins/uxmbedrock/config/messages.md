---
title: Messages
order: 610
description: How a line is written, the prefixes, and every block of keys.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A key missing from a file falls back to the default language, then to the line
the code carries.

| Prefix | Draws |
|---|---|
| `<tag:'BEDROCK'>` | The category word of a reply |
| `<etag:'ERROR'>` | An error |
| `<h:'...'>` | A header |

## Blocks

| Block | Said when |
|---|---|
| `admin` | `/uxmbedrock`: reload, version, `status`, `pack`, `report`, `sync`, `items`, `skulls`, `menus`, `preview`, `doctor` |
| `player` | Replies to `/bedrock` and `/menus`: not on Bedrock, menus, features, the animated pack |
| `words` | The click, tap and press words: `click-words` lists every way this language writes click, separated by commas |
| `forms` | The sign and anvil forms |
| `settings` | The settings form; `settings.features.<feature>` names each feature |
| `animated` | The first-join question |
| `gui` | `gui measure` and `gui list` |
| `language` | `/bedrock lang` and the operator's language commands |
| `command` | The command layer: no permission, wrong value, help |

The words of `words` are read as written, without the small capitals the theme gives other lines, since they are
matched against other plugins' text.

Related: [Languages](languages.md), [theme.conf](theme.md)
