---
title: Configuration
order: 50
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, network, effects, window sounds |
| [commands.conf](commands-conf.md) | The name, aliases and switch of both commands |
| [languages.conf](languages-conf.md) | The name and icon of each language |
| [Menu file](menu-files.md) | `menus/language.conf` |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and adding one |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## Reloading

`/uxmlang reload` reads `config.conf`, `languages.conf`, the window and the language files again and swaps them in
whole, so a reload is never half applied. `storage`, `network` and `commands.conf` need a restart.

## What an update does

When a release adds a key, the next start adds it to your `config.conf` with the comment that explains it, and it never
changes a value you wrote. Your file as it was is kept in `backups/`; the last five copies are kept. A key you deleted
comes back at its default, which is why every switch says what turning it off means.

Related: [config.conf](config-conf.md)
