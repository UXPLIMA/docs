---
title: Configuration
order: 50
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Every block, with its keys and defaults |
| [roles.conf](roles-conf.md) | The roles every new claim starts with |
| [commands.conf](commands-conf.md) | Command and branch words, per language |
| [Menu files](menu-files.md) | The fourteen windows |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

Every file is HOCON. `/claimadmin reload` reads them again; `storage`, `network` and `commands.conf` need a restart.
When a release adds a key to `config.conf`, the next start adds it with its comment and keeps your values; the file as
it was goes into `backups/`.

Related: [config.conf](config-conf.md)
