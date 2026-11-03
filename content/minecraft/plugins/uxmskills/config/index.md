---
title: Configuration
order: 150
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Every block, with its keys and defaults |
| [anti-farm.conf](anti-farm-conf.md) | The anti-farm terms |
| [economy.conf](economy-conf.md) | Money backends for costs and requirements |
| [boosts.conf](boosts-conf.md) | Rank multipliers and the boost calendar |
| [Content files](content-files.md) | `skills/`, `sources/`, `abilities/`, `stats/`, `resources/` |
| [commands.conf](commands-conf.md) | Names, aliases and the skill shortcuts |
| [Menu files](menu-files.md) | The thirteen windows |
| [text-input.conf](text-input-conf.md) | How typing is asked for |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

`/uxmskills reload` reads every file again, and a refused file names itself and the position. `storage`, `commands.conf`
and `claims.providers` need a restart. When a release adds keys, the next start adds them with their comments and keeps
your values; the file as it was goes into `backups/`.

Related: [config.conf](config-conf.md)
