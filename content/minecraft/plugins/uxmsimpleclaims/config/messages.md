---
title: Messages
order: 505
description: How a line is written and where the window words live.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A key that is missing falls back to the default the plugin carries. Window
titles, buttons and tiles live under `menu.*`; role, flag and permission words are translated there too, so a Turkish
reader sees Turkish role names.

Related: [Languages](languages.md), [theme.conf](theme.md)
