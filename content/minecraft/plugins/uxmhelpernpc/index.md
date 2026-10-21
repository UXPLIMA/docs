---
title: uxmHelperNPC
order: 200
description: A per-player helper NPC that stands on the player's own island or claim
  and opens the menus you give it.
icon: bot
---

uxmHelperNPC gives every player one NPC of their own. It is placed on their island or claim (either automatically when the region is created, or by the player buying it)
and clicking it
runs whatever you configured: a menu, a command, a sound, a question to an AI assistant.

The plugin owns no world of its own. It asks a claim plugin where a player's region is, an NPC
plugin how to draw a character, and an economy plugin what it costs. You pick each of those in
`config.yml`.

- [Getting Started](getting-started/)
- [Commands](commands.md)
- [Features](features/)
- [Menus](menus/)
- [Configuration](config/)
- [Integrations](integrations.md)
- [Database](database.md)
