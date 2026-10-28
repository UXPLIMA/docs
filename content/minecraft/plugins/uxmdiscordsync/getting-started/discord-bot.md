---
title: Creating the bot
order: 11
description: The Discord application, the two intents it needs, its permissions and where the ids come from.
icon: bot
---

The bot is a Discord application you create once. The plugin logs in as it; no second program has
to run anywhere.

## The application

1. Open the [Discord Developer Portal](https://discord.com/developers/applications) and create a new
   application.
2. Open **Bot**, reset the token and copy it. It goes into `discord.token` in `config.yml`. Treat it
   like a password: whoever has it controls the bot.
3. On the same page, under **Privileged Gateway Intents**, turn on **Server Members Intent** and
   **Message Content Intent**.
4. Open **OAuth2**, then **URL Generator**. Tick the scopes `bot` and `applications.commands`, tick
   the permissions from the table below, and open the generated link to add the bot to your Discord
   server.

<Callout type="warning" title="Both intents are required">

The plugin always asks Discord for both intents. With either one switched off in the portal, Discord
refuses the login, the console shows `Failed to start Discord bot`, and every Discord feature stays
off until the intent is on and the server restarts.

</Callout>

## Permissions

| Permission | Needed for |
|---|---|
| View Channels, Send Messages, Embed Links | Everything the bot posts |
| Read Message History | The link channel. The bot looks for its own link message there and does not post one without this |
| Manage Roles | The linked roles and rank sync |
| Manage Nicknames | Nickname sync |
| Connect | Parking the bot in a voice channel |

The bot's own role must sit above every role it gives and above every member whose nickname it
changes. Discord refuses both otherwise. Move the bot's role near the top of the role list in
**Server Settings**, then **Roles**.

Nobody can change the server owner's nickname, a bot included.

The bot also sends direct messages: the 2FA request, the message after linking and the level-up
message. A member who blocks direct messages from server members does not receive them, and for 2FA
that means their login times out.

## Getting ids

In Discord, open **User Settings**, then **Advanced**, and turn on **Developer Mode**. Right-clicking
a server, channel, role or user then offers **Copy ID**.

| Id | Where it goes |
|---|---|
| Your Discord server | `discord.guild-id` in `config.yml` |
| The link channel | `discord.linking.channel-id` |
| Roles every linked member gets | `discord.linking.linked-role-ids` |
| Roles for each rank | `rank-mappings` in `rank-sync.yml` |
| The chat bridge channel | `chat-bridge.discord-channel-id` |
| The stats channel | `server-stats.channel-id` |
| Staff allowed to use `/servercommand` and `/broadcast` | `authorized-users` in `config.yml` |
| Channels that earn XP | `text-level.channels` and `voice-level.channels` in `level-system.yml` |

Keep ids in quotes: `"123456789012345678"`. Written without them, YAML reads an 18-digit id as a
number and can change its last digits.

The bot only works in the Discord server named by `discord.guild-id`. If you add it to other
servers, it ignores them.

## Webhooks

The logs are not posted by the bot but through webhooks, so a busy log never slows the bot down. Make
one webhook per log channel: **Edit Channel**, **Integrations**, **Webhooks**, **New Webhook**, then
**Copy Webhook URL**. Paste the URL into the matching `webhook-url` in `logging.yml`; see
[Logging](../features/logging.md).

A webhook URL is a password too: anyone who has it can post in that channel. If one leaks, delete the
webhook in Discord and make a new one.
