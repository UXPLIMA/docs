---
title: Configuration
order: 50
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, item data, limit, routes, drag and drop, anvil, grindstone, worlds, books, display, charges, cooldowns, the treaty, effect materials, claims, editor, menu sounds |
| [Enchantment files](../features/enchantment-files.md) | `enchantments/<id>.conf`, one per enchantment |
| [rarities.conf](rarities-conf.md) | The chance on each route and the table level |
| [types.conf](types-conf.md) | The format, the per item limit, the grindstone rule |
| [targets.conf](targets-conf.md) | Extra enchantable items, named sets, targets and slots |
| [vanilla.conf](vanilla-conf.md) | Vanilla enchantments in your own words |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | The six windows |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

Every file is HOCON: `key = value`, blocks in braces, `#` comments.

## Reloading

`/uxmenchants reload` reads `config.conf`, every content file, the messages, the theme and the three player windows
again, and redraws every player's items. The content is validated whole first; a wrong file refuses the reload and
names the file, the key and the line. `storage`, `editor.enabled` and `commands.conf` need a restart.

## What an update does

When a release adds a key, the next start adds it to your `config.conf` with the comment that explains it, and it never
changes a value you wrote. Your file as it was is kept in `backups/`; the last five copies are kept. Alignment inside a
block may move, and a key you deleted comes back. The other files are written only when missing.

Related: [config.conf](config-conf.md)
