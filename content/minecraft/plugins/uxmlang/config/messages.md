---
title: Messages
order: 605
description: How a line is written, the prefixes, and every block of keys with the values each fills in.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A key that is missing falls back to the default the code carries, so a line is
never empty. Delete a line to take the default back.

| Prefix | Draws |
|---|---|
| `<tag:'LANGUAGE'>` | The plugin's category word |
| `<etag:'ERROR'>` | An error |
| `<h:'...'>` | A header |
| `<caps>...</caps>` | Small capitals, for a language's name |

| Block | Values | Said when |
|---|---|---|
| `admin.version`, `usage`, `reloaded`, `reload-failed` | `<version>`, `<ms>` | The operator commands |
| `admin.doctor.*` | `<check>`, `<said>` | `/uxmlang doctor` |
| `error.menu-missing` | `<menu>` | The window could not be read |
| `language.current`, `choices` | `<language>`, `<languages>` | `/lang list` |
| `language.changed`, `already`, `reset` | `<language>` | The effects' lines |
| `language.unknown`, `forced` | `<input>`, `<languages>`, `<language>` | Refusals |
| `language.set-other`, `of-other`, `reset-other` | `<player>`, `<language>` | `/uxmlang set`, `get`, `reset` |
| `language.separator` | | Between two languages in a list |
| `menu.language.*` | `<language>`, `<state>` | The window: title, tile, follow button, states |
| `menu.button.previous`, `next` | | The page arrows |
| `command.*` | | The command layer: help pages, rejected arguments |

Related: [Languages](languages.md), [theme.conf](theme.md)
