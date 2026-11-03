---
title: Configuration
order: 50
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

Four kinds of file, and each one owns a different decision. A value that changes behaviour is in `config.conf`, a word
a player reads is in a language file, a window is a file of its own, and a colour of the interface is in the shared
theme.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, cycle, effects, window sounds |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | `menus/glow.conf`: the picker |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## Reloading

`/uxmglow reload` reads `config.conf`, the picker and the language files again and swaps them in whole, so a reload is
never half applied. `storage` and `commands.conf` need a restart.

## What an update does

When a release adds a key, the next start adds it to your file with the comment that explains it, and it never changes
a value you wrote. Your file as it was is kept in `backups/` as `config-<date>.conf`; the last five copies of each file
are kept. A key you deleted comes back at its default.

Related: [config.conf](config-conf.md)
