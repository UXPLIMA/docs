---
title: Messages
order: 607
description: The language files, messages/messages_<language>.conf, and how a line is written.
icon: message-square
---

Every line a player reads is in `messages/messages_<language>.conf`, one file per language. No line is written in
code. `/is reload` reads the files again.

## How a line is written

A line is MiniMessage with the roles of [theme.conf](theme.md), so it takes the server's colours:

| Tag | What it is |
|---|---|
| `<tag:'ISLAND'>` | A small label in front of a line |
| `<etag:'ERROR'>` | The label of an error line |
| `<body>`, `<value>`, `<muted>`, `<good>`, `<bad>`, `<warn>`, `<money>`, `<level>`, `<accent>` | Colour roles |
| `<h:'Title'>` | A heading |
| `<cmd:'home'>` | A command word, written as the player's own word for it from `commands.conf` |
| `<plain>...</plain>` | Text drawn as written, never in small capitals |
| `<player>`, `<amount>`, `<island>` and others | Values the line is given; each line names its own |

A key in [presets.conf](presets-conf.md), [missions.conf](missions-conf.md) and other files that starts with `@` is
looked up in these files, so every language answers it in its own words.

## Updating

A release that changes a line you never edited updates it on your server. A line you edited stays yours. A key missing
from a file falls back to English for the players reading that language.
