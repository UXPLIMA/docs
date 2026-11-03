---
title: Configuration
order: 50
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, entries, player count, hover, version text, centring, icons, maintenance |
| [commands.conf](commands-conf.md) | The name, aliases and switch of the command and each word |
| [Messages](messages.md) | `messages/messages_<language>.conf`: replies and the kick screen |
| [Languages](languages.md) | The seven languages and how a viewer's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## Reloading

`/uxmmotd reload` reads `config.conf`, the theme, the messages and the icons again and swaps them in as one snapshot. When
a file is invalid the previous configuration stays in use and the console holds the reason. `commands.conf` needs a
restart.

## What an update does

When a release adds a key, the next start adds it to your `config.conf` with the comment that explains it, and it never
changes a value you wrote. Your file as it was is kept in `backups/`; the last five copies are kept.

Related: [config.conf](config-conf.md)
