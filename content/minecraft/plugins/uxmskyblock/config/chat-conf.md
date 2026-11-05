---
title: chat.conf
order: 619
description: Island chat, ally chat and who may listen in.
icon: message-square
---

Island chat: /is chat talks to the island's members only, /is allychat to its allies too.

The file is `plugins/uxmSkyblock/modules/chat.conf`. Every default below is the shipped value.

## `chat`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | false turns island chat off. |
| `format` | `<tag:'Island'> <muted>[<role>] <value><player><muted>: <body><message>` | The line an island chat message is drawn in. Write a role of theme.conf, such as `<muted>` or `<value>`, rather than a colour, and `<tag:'Word'>` for the category in front. Placeholders: `<role>`, `<player>`, `<message>` |
| `alliance-format` | `<tag:'Alliance'> <muted>[<role>] <value><player><muted>: <body><message>` | The same for a message to the island's alliance. |
| `spy-format` | `<tag:'Spy'> <muted>[<accent><island_name><muted>] [<role>] <value><player><muted>: <body><message>` | The line a member of staff spying on island chat reads. Placeholders: `<role>`, `<player>`, `<message>`, `<island_name>` |
| `rate-limit-messages-per-second` | `5` | Maximum messages allowed per second per player before rate limiting kicks in (0 to disable). |
