---
title: Installation
order: 12
description: Requirements, first run, and turning features on one at a time.
icon: download
---

## Requirements

| | |
|---|---|
| Server | Paper 1.21 or newer |
| Java | 21 or newer |
| Required | A Discord bot token and guild id |
| Recommended | LuckPerms (rank sync), PlaceholderAPI (custom commands) |

JDA, HikariCP, the database drivers, Caffeine and the webhook client are declared as libraries in
`plugin.yml` and downloaded by the server at startup. Nothing needs to be installed by hand.

## First run

1. Drop `uxmDiscordSync.jar` into `plugins/` and start the server. It will start, write its
   configuration, and report that no token is set.
2. Stop the server.
3. In `config.yml`, set:
   - `plugin.license-key`, from your UXPLIMA panel
   - `discord.token`: the bot token
   - `discord.guild-id`: your guild's id
   - `discord.linking.channel-id`: where the link embed is posted
   - `discord.linking.linked-role-ids`: the roles a linked player receives
   - `plugin.server-name`: a unique name if you run more than one server
4. Start the server. The console should report the guild name it connected to and how many slash
   commands it registered.

## Linking the first account

```
/link
```

in Minecraft returns a six-digit code. In Discord, the linking channel now holds an embed with a
**Link Account** button; clicking it opens a modal that takes the code.

If the embed is not there, the bot could not post in that channel: check that it can see the
channel and has Embed Links.

## Turning features on

Every feature ships off or empty, and each lives in its own file:

| File | Feature |
|---|---|
| `config.yml` | Bot, linking, chat bridge, server stats, broadcast, server commands |
| `rank-sync.yml` | Rank and nickname sync |
| `two-factor.yml` | Login verification |
| `level-system.yml` | Discord levels and rewards |
| `boost-rewards.yml` | Boost rewards |
| `punishment-sync.yml` | Punishment sync |
| `logging.yml` | Chat, join/leave and audit logs |
| `commands.yml` | Minecraft command names and permissions |
| `advanced.yml` | Threads, cache, retries, cleanup |
| `discord-commands/` | One file per custom Discord command |

Turn them on one at a time and check each works before adding the next. A misconfigured role id in
rank sync and a missing intent produce the same symptom, and it is much easier to tell them apart
when only one thing changed.

## Reloading

```
/uxmdiscordsync reload
```

Re-reads the configuration files. The bot connection, its intents and the registered slash commands
are established at startup: changing `discord.token`, `discord.guild-id` or adding a file to
`discord-commands/` needs a restart.
