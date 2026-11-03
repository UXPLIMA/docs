---
title: Configuration
order: 50
description: Every file the plugin writes, reloading, and what an update does.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, face, bedrock, skins, welcome |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Messages](messages.md) | The welcome lines and every reply |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

`/uxmheads reload` reads `config.conf` and the messages again and swaps them in whole. `commands.conf` needs a
restart.

When a release adds a key, the next start adds it with its comment and never changes a value you wrote; the file as it
was is kept in `backups/`, the last five copies.

Related: [config.conf](config-conf.md)
