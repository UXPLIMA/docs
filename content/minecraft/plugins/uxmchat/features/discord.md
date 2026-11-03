---
title: Discord
order: 320
description: Bridging channels to Discord by webhook or bot, posting events and staff actions, and the /online and /playerlist commands.
icon: message-square-share
---

The Discord add-on is its own jar, `uxmChat-Discord-<version>.jar`, beside uxmChat in `plugins/`. Start once and fill
in `plugins/uxmChat-Discord/discord.conf`.

## What it does

| Part | Does |
|---|---|
| Bridges | Each uxmChat channel joined to a Discord channel, both ways |
| Events | Joins, first joins, quits, deaths, kicks, advancements and network lines posted to one channel |
| Moderation log | Staff actions uxmChat records (mutes, deletes, clears, locks) posted to one channel |
| Slash commands | `/online` (how many are online) and `/playerlist` (who, by server) |

A webhook posts a line as the player, with their head as the avatar. A bot token brings Discord messages into the game
and answers the slash commands; without a webhook the bot posts with the `bot-line` text.

## Where to run it

Put it on one server of a network and keep `post-network-lines = true`, or on every server with it `false`, so each
server posts what was said on it. Run the bot (the token) on one server only.

## Setting it up

1. Create a bot in the Discord developer portal and turn on the **Message Content** intent.
2. Put its token in `bot.token` and your guild id in `bot.guild`. An empty guild registers the slash commands
   everywhere, which Discord can take up to an hour to show.
3. For each bridge, write the uxmChat `channel`, the `discord-channel` id and, to post as players, a `webhook`.
4. Restart.

<Callout type="warning" title="Secrets">

The bot token and webhook URLs are secrets: never put `discord.conf` in git. `env:NAME` reads one from an environment
variable instead: `token = "env:UXMCHAT_DISCORD_TOKEN"`.

</Callout>

Related: [discord.conf](../config/discord-conf.md), [A network](network.md)
