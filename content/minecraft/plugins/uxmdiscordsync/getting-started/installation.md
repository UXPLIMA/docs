---
title: Installation
order: 12
description: Requirements, the first start, the files you fill in and what ships turned on.
icon: download
---

## Requirements

| | |
|---|---|
| Server | Paper 1.21 or newer, or a fork of Paper. Spigot is not supported |
| Java | 21 or newer |
| License | A uxmDiscordSync license key from UXPLIMA |
| Discord | A bot made as in [Creating the bot](discord-bot.md) |
| Optional | LuckPerms for rank sync, PlaceholderAPI for placeholders from other plugins |
| Only for several servers | Redis 5.0 or newer and a shared MySQL, MariaDB or PostgreSQL database. See [Multi-server](../features/multi-server.md) |

The libraries the plugin uses (the Discord library, the database drivers, the Redis client) are
downloaded by Paper on the first start, so that start needs internet access and takes longer.

## First start

1. Put the jar into `plugins/` and start the server. The plugin writes its files into
   `plugins/uxmDiscordSync/` and stops, because no license key is set yet.
2. Stop the server.
3. Fill in `config.yml`:

| Setting | Value |
|---|---|
| `plugin.license-key` | Your license key. If you do not have one, open a ticket in the UXPLIMA Discord |
| `plugin.server-name` | A short name for this server. Letters, digits, spaces, `-` and `_`, at most 50 characters |
| `discord.token` | The bot token |
| `discord.guild-id` | The id of your Discord server |
| `discord.linking.channel-id` | The channel where the link button is posted |
| `discord.linking.linked-role-ids` | Roles every linked member gets, or `[]` for none |

4. Start the server. The console shows `Connected to Discord guild:` with your server's name, then
   `Persistent link embed sent to channel:` with the link channel's name.

`server-name` is read once, when the server starts: a change needs a restart.

## Linking the first account

Join the server and type `/link`. You get a six-digit code in chat; clicking it copies it. In
Discord, open the link channel, press **Link Account**, type the code into the window that opens and
send it. The bot answers with a confirmation that only you can see, and the linked roles are added.

If the link channel stays empty, the bot cannot see it or lacks **Read Message History** there. The
console names the reason.

## What ships turned on

| File | Feature | Ships |
|---|---|---|
| `config.yml` | Linking, bot status | On |
| `config.yml` | Voice channel, server stats, chat bridge, `/servercommand`, `/broadcast`, multi-server | Off |
| `rank-sync.yml` | Rank sync and nickname sync | On, but rank sync needs LuckPerms and real role ids |
| `boost-rewards.yml` | Boost rewards | On |
| `logging.yml` | Chat, join and leave, and audit logs | On, but nothing is posted until you set the webhook URLs |
| `discord-custom-commands.yml` | Six example commands such as `/ip` and `/store` | On |
| `two-factor.yml` | Login approval | Off |
| `level-system.yml` | Discord levels | Off |

Nickname sync is on from the start: every linked player is renamed in Discord to
`Name (@server-name)`. Change `nickname-sync.format` first, or turn it off, if that is not what you
want. See [Rank and nickname sync](../features/rank-sync.md#nickname-sync).

The logs warn once per startup about each webhook URL still set to `YOUR_WEBHOOK_URL_HERE`. Fill them
in or set `enabled: false` for the logs you do not want.

Turn features on one at a time and check each one before the next. A wrong role id and a missing
permission look the same in Discord, and are much easier to tell apart when only one thing changed.

## Reload or restart

`/dsadmin reload` reads every file again. If a file has a YAML error, nothing is reloaded and the
command names the file and the line.

| Change | Reload is enough | Needs a restart |
|---|---|---|
| Messages, embeds, rewards, rank mappings, webhook URLs | Yes | |
| Turning features on or off | Yes | |
| Custom Discord commands, added or changed | Yes | |
| Command names and aliases in `commands.yml` | Yes | |
| `discord.token`, `discord.guild-id` | | Yes |
| `plugin.server-name`, `database`, `multi-server` | | Yes |

A YAML error at startup stops the plugin instead of starting it on defaults, so the token and the
license are never overwritten. The console names the file and the line.
