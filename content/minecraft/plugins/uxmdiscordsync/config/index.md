---
title: Configuration
order: 50
description: Every file in the plugin folder, what belongs in it, and which page explains it.
icon: settings
---

```
plugins/uxmDiscordSync/
├── config.yml                    license, bot, database, linking, several servers, bridge, stats
├── commands.yml                  game command names, aliases, permissions and cooldowns
├── rank-sync.yml                 rank and nickname sync
├── two-factor.yml                login approval
├── level-system.yml              Discord levels and their rewards
├── boost-rewards.yml             boost rewards
├── logging.yml                   chat, join and leave, and audit webhooks
├── advanced.yml                  cache, link code limit, cleanup
├── discord-custom-commands.yml   the switch for every custom command
├── discord-commands/             one file per custom Discord command
├── languages/                    en_US.yml and tr_TR.yml
├── database.db                   the SQLite database, written by the plugin
├── data/                         written by the plugin: license cache, saved state, flatfile data
└── backups/                      copies of the old files, made when a new version updates them
```

| File or section | Page |
|---|---|
| `config.yml`: `plugin`, `discord`, `linking`, `localization`, `discord-server-command`, `discord-broadcast` | [config.yml](config-yml.md) |
| `config.yml`: `database` | [Database](database.md) |
| `config.yml`: `multi-server` | [Multi-server](../features/multi-server.md) |
| `config.yml`: `server-stats`, `discord.presence`, `discord.voice` | [Server stats and presence](../features/server-stats.md) |
| `config.yml`: `chat-bridge` | [Chat bridge](../features/chat-bridge.md) |
| `commands.yml` | [Commands](../commands.md#changing-a-command) |
| `rank-sync.yml` | [Rank and nickname sync](../features/rank-sync.md) |
| `two-factor.yml` | [Two-factor login](../features/two-factor.md) |
| `level-system.yml` | [Level system](../features/level-system.md) |
| `boost-rewards.yml` | [Boost rewards](../features/boost-rewards.md) |
| `logging.yml` | [Logging](../features/logging.md) |
| `advanced.yml` | [advanced.yml](advanced.md) |
| `discord-custom-commands.yml`, `discord-commands/` | [Custom Discord commands](custom-commands.md) |
| `languages/` | [Languages](languages.md) |

Do not edit `config-version` at the bottom of `config.yml`: the plugin uses it to update your files
when you install a new version.

A file with a YAML error stops the plugin at startup, and refuses `/dsadmin reload`, instead of
running on defaults. The console names the file and the line. Deleting a file gives you a fresh copy
of it on the next start.

- [config.yml](config-yml.md)
- [Database](database.md)
- [Custom Discord commands](custom-commands.md)
- [advanced.yml](advanced.md)
- [Languages](languages.md)
