---
title: Configuration
order: 50
description: Every file in the plugin folder and what belongs in it.
icon: settings
---

```
plugins/uxmDiscordSync/
├── config.yml                    bot, linking, bridge, stats, broadcast
├── commands.yml                  Minecraft command names and permissions
├── rank-sync.yml                 rank and nickname sync
├── two-factor.yml                login verification
├── level-system.yml              Discord levels and rewards
├── boost-rewards.yml             boost rewards
├── punishment-sync.yml           punishment sync
├── logging.yml                   chat, join/leave, audit webhooks
├── advanced.yml                  threads, cache, retries, cleanup
├── discord-custom-commands.yml   the master switch for custom commands
├── discord-commands/             one file per custom command
└── languages/
    ├── en_US.yml
    └── tr_TR.yml
```

- [config.yml](config-yml.md)
- [Database](database.md)
- [Custom Discord commands](custom-commands.md)
- [advanced.yml](advanced.md)
- [Languages](languages.md)
