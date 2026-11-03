---
title: discord.conf
order: 619
description: "Every key of the Discord add-on: the bot, bridges, events, the moderation log, the avatar and the texts."
icon: message-square-share
---

`plugins/uxmChat-Discord/discord.conf`. Never put it in git; `env:NAME` reads a secret from an environment variable.

| Key | Default | Meaning |
|---|---|---|
| `bot.token` | `""` | The bot's token, for lines from Discord and the slash commands; empty posts by webhook only |
| `bot.guild` | `""` | The guild id the slash commands are registered in; empty registers them everywhere |
| `post-network-lines` | `true` | Also post lines of network channels and network joins and quits said on other servers |
| `bridges` | `global`, both ways | Each `{ channel, discord-channel, webhook, to-game, to-discord }` |
| `events.discord-channel`, `webhook` | `""` | Where events are posted |
| `events.kinds` | `join`, `first-join`, `quit`, `death`, `advancement`, `network-join`, `network-quit` | Also `kick` and `network-switch` |
| `moderation-log.discord-channel`, `webhook` | `""` | Where staff actions are posted |
| `avatar` | `https://mc-heads.net/avatar/{uuid}/64` | A webhook post's avatar |
| `max-length` | `256` | The longest line from Discord that goes into the game |

## texts

| Key | Default |
|---|---|
| `source` | `Discord` |
| `bot-line` | `**{name}**: {text}` |
| `moderation` | `**{staff}** {action} {target} {detail}` |
| `online-description` | `How many players are online` |
| `playerlist-description` | `Who is online, by server` |
| `online` | `{count} players online: {servers}` |
| `online-server` | `{server} {count}` |
| `playerlist-server` | `**{server}** ({count}): {names}` |
| `nobody-online` | `Nobody is online.` |

Related: [Discord](../features/discord.md)
