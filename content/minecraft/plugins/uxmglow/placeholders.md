---
title: Placeholders
order: 50
description: What uxmGlow answers on a scoreboard, and what an empty value means.
icon: hash
---

Registered only when PlaceholderAPI is on the server, and taken down again when the plugin is
disabled, so a reload never leaves a stale expansion answering.

| Placeholder | Value |
|---|---|
| `%uxmglow_glowing%` | `true` or `false` |
| `%uxmglow_mode%` | `solid`, `cycle`, or empty |
| `%uxmglow_colour%` | The readable name of the colour, or empty |
| `%uxmglow_colour_id%` | The id of the colour, or empty |
| `%uxmglow_tag%` | The MiniMessage tag of the colour, or empty |

A player who is not glowing gives an **empty** value and not a word. An empty value disappears
from a scoreboard line, and a word such as `none` would need translating, which a placeholder
cannot do: it does not know which language the viewer reads.

A cycling player answers with the first colour of their cycle. A placeholder is asked once and
cannot animate.

An unknown parameter answers null, so PlaceholderAPI leaves the text exactly as you wrote it.
That is how you find a typo in your own scoreboard.
