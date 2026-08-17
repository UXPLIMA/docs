---
title: Creating the bot
order: 11
description: Creating the application, the intents it needs, and where the IDs come from.
icon: bot
---

## The application

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications) and create a
   new application.
2. Open **Bot** and copy the token. This is the value for `discord.token`: treat it as a password.
3. On the same page, enable the **Privileged Gateway Intents** the plugin needs:
   - **Server Members Intent**: required for rank sync, nickname sync and member join/leave
     logging.
   - **Message Content Intent**: required for the chat bridge, text-level XP and chat-style
     custom commands (`!ip`).
4. Open **OAuth2 → URL Generator**, tick `bot` and `applications.commands`, pick the permissions
   below, and use the generated URL to invite the bot.

<Callout type="warning" title="Missing intents fail quietly">

Without the Server Members intent, rank sync appears to run and assigns nothing. Without Message
Content, the chat bridge sees empty messages. Neither produces an obvious error, if a feature does
nothing at all, check the intents first.

</Callout>

## Bot permissions in the guild

| Permission | Needed for |
|---|---|
| View Channels, Send Messages, Embed Links | Everything |
| Manage Roles | Rank sync |
| Manage Nicknames | Nickname sync |
| Ban Members, Kick Members, Moderate Members | Punishment sync |
| Connect | The voice-channel presence feature |
| Use Application Commands | Slash commands |

The bot's own role must sit **above** every role it assigns and above every member whose nickname it
changes. Discord refuses both operations otherwise, and it refuses them silently: a bot cannot
manage a role equal to or higher than its own.

The guild owner's nickname can never be changed by a bot. `nickname-sync.skip-on-permission-error:
true` is what stops that from filling your console.

## Getting IDs

Turn on **Settings → Advanced → Developer Mode** in your Discord client. Right-clicking anything then
offers **Copy ID**. You need:

| Id | Where it goes |
|---|---|
| Guild | `discord.guild-id` |
| Linking channel | `discord.linking.channel-id` |
| Linked roles | `discord.linking.linked-role-ids` |
| Rank roles | `rank-sync.yml → rank-mappings` |
| Chat bridge channel | `chat-bridge.discord-channel-id` |
| Stats channel | `server-stats.channel-id` |
| Your own user | `discord-server-command.authorized-users` |

Ids are strings. Keep the quotes: an 18-digit number written without them loses precision in YAML.

## Webhooks

Logging uses webhooks rather than the bot, so log volume does not consume the bot's rate limit.
Create one per channel under **Channel → Edit Channel → Integrations → Webhooks**, copy the URL, and
paste it into the matching `webhook-url` in `logging.yml`.

A webhook URL is a credential: anyone holding it can post to that channel as the webhook. Do not
commit `logging.yml` to a public repository.
