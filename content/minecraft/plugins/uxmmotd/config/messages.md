---
title: Messages
order: 603
description: The command replies and the kick screen, one file per language.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours. A key that
is missing falls back to `language.default`, then to the line the code carries. The server list entry is not here: it is
your own writing in `config.conf`.

| Prefix | Draws |
|---|---|
| `<tag:'MOTD'>` | The plugin's category word |
| `<etag:'ERROR'>` | An error |
| `<h:'...'>` | A header |

| Block | Values | Said when |
|---|---|---|
| `admin.version`, `usage`, `reloaded`, `reload-failed` | `<version>`, `<ms>` | The commands |
| `admin.doctor.*` | `<check>`, `<said>` | `/uxmmotd doctor` |
| `maintenance.turned-on`, `turned-off`, `already-on`, `already-off`, `status-on`, `status-off` | | `/uxmmotd maintenance` |
| `maintenance.kick` | | The screen a refused player reads |
| `command.*` | | The command layer: help pages, rejected arguments |

A value inserted into a line is shown as written and never obeyed as markup.

Related: [Languages](languages.md), [theme.conf](theme.md)
