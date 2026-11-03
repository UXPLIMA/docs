---
title: Configuration
order: 50
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, house rules, history, events, webhook, placeholders, asking, item lines, admin, storage, network, fees, effects, menu sounds |
| [currencies.conf](currencies-conf.md) | The currencies and how amounts are written |
| [categories.conf](categories-conf.md) | The tabs of the house |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | The fourteen windows |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## Reloading

`/uxmauction reload` reads every file again and swaps them in as one snapshot. `storage`, `network` and `commands.conf`
need a restart.

## What an update does

When a release adds a key, the next start adds it to your `config.conf` with the comment that explains it, and it never
changes a value you wrote. Your file as it was is kept in `backups/`; the last five copies are kept.

Related: [config.conf](config-conf.md)
