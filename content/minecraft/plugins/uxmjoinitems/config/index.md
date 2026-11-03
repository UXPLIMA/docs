---
title: Configuration
order: 50
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, the default cooldown, the proxy channel |
| [Item files](item-files.md) | `items/<id>.conf`, one per item |
| [Event files](event-files.md) | `events/<event>.conf`, one per moment |
| [commands.conf](commands-conf.md) | The name, aliases and switch of the command and each subcommand |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## Reloading

`/uxmjoinitems reload` reads `config.conf`, every item and event file and the messages again and swaps them in as one
snapshot, so a reload is never half applied. `commands.conf` needs a restart.

## What an update does

When a release adds a key, the next start adds it to your `config.conf` with the comment that explains it, and it never
changes a value you wrote. Your file as it was is kept in `backups/`; the last five copies are kept.

Related: [config.conf](config-conf.md)
