---
title: Configuration
order: 150
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, this lobby, status, cooldown, menu, hotbar tool, effects |
| [lobbies.conf](lobbies-conf.md) | The lobbies of the network |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu file](menu-files.md) | `menus/lobbies.conf` |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## Reloading

`/uxmmultilobby reload` reads every file again and swaps them in as one snapshot. A value that will not parse leaves the
previous snapshot in place and the server keeps running. `commands.conf` needs a restart.

## What an update does

When a release adds a key, the next start adds it to your `config.conf` with the comment that explains it, and it never
changes a value you wrote. Your file as it was is kept in `backups/`; the last five copies are kept. A key you deleted
comes back at its default. The lobby list is the exception and stays yours: a lobby you removed is not written back.

Related: [config.conf](config-conf.md)
