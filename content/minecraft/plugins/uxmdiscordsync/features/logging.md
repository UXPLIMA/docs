---
title: Logging
order: 38
description: The chat log, join and leave logs and the audit log, each posted through its own webhook.
icon: scroll-text
---

Three logs, all in `logging.yml`, all posted through Discord webhooks instead of the bot. Each has
its own `webhook-url`, so each can go to a channel with its own permissions. How to make a webhook is
on [Creating the bot](../getting-started/discord-bot.md#webhooks).

A webhook URL must start with `https://discord.com/api/webhooks/`. A channel link, or the default
`YOUR_WEBHOOK_URL_HERE`, turns that log off with a warning in the console.

## Chat log

```yaml
chat-logging:
  enabled: true
  webhook-url: "https://discord.com/api/webhooks/..."
  format: "**[%server_name%]** %player%: %message%"
  use-embed: false
  filter:
    ignore-prefixes: ["/", "!", "#"]
    min-length: 1
    max-length: 500
    ignored-permission: ""
  rate-limit:
    enabled: true
    max-messages-per-minute: 30
```

| Setting | Meaning |
|---|---|
| `format` | One line per message. `%player%`, `%player_displayname%`, `%message%`, `%server_name%`, `%world%`, `%timestamp%` |
| `use-embed` | `true` posts an embed from `embed` instead, which also takes `%player_uuid%` |
| `ignore-prefixes` | Messages starting with one of these are not logged |
| `min-length`, `max-length` | Messages shorter or longer are not logged at all. A long message is skipped, not cut |
| `ignored-permission` | Players with this permission are not logged. Empty turns it off |
| `max-messages-per-minute` | Messages past this many in a minute are not logged |

A message another plugin cancels, such as a muted player's, is not logged. Discord lets a webhook
post 30 messages a minute; a higher limit only means Discord drops them instead.

Plain lines are denser and easier to search, embeds easier to read. For a log you search through,
plain lines win.

## Join and leave

Two separate logs: `join-leave-logging.minecraft` for players on the server and
`join-leave-logging.discord` for members of your Discord server. Each has its own webhook and its
own `join` and `leave` texts, as a plain `message` or, with `use-embed: true`, an `embed`.

| Log | Placeholders |
|---|---|
| Minecraft | `%player%`, `%player_displayname%`, `%player_uuid%`, `%server_name%`, `%online_players%`, `%max_players%` |
| Discord | `%user%`, `%user_id%`, `%user_avatar_url%`, `%member_count%` |

The counts are the ones after the join or leave.

For the Minecraft log, keep `use-embed: true`. With `false`, the plugin posts a fixed line, the
player's name followed by `joined the server` or `left the server`, and does not read your `message`.

With several servers, every server logs its own players, and Discord members are logged once.

## Audit log

The audit log is the record you go back to when something is disputed: who linked to which Discord
account and when, who claimed a boost reward, which roles a sync gave.

```yaml
audit:
  enabled: true
  account-link:
    enabled: true
    webhook-url: "https://discord.com/api/webhooks/..."
    embed: ...
  account-unlink: ...
  boost-claim: ...
  rank-sync: ...
```

`audit.enabled: false` turns all four off.

| Event | Posted when | Placeholders |
|---|---|---|
| `account-link` | An account is linked, also with `forcelink` | `%player_name%`, `%player_uuid%`, `%discord_user%`, `%discord_id%`, `%server_name%`, `%timestamp%` |
| `account-unlink` | A link is removed | The same |
| `boost-claim` | A boost reward is claimed | The same |
| `rank-sync` | A sync changed someone's roles | `%player_name%`, `%player_uuid%`, `%minecraft_ranks%`, `%discord_roles%`, `%server_name%`, `%timestamp%` |

`%minecraft_ranks%` lists the player's groups at the time and `%discord_roles%` the roles they map
to, which turns "the plugin took my role" into a question with an answer.

Send the audit log to a staff-only channel: player ids and Discord ids belong there, not in public.

Links, unlinks, boost claims and rank syncs are also kept in the database, and the plugin never
deletes them.

## Sending and retrying

| Setting in `config.yml` | Default | Meaning |
|---|---|---|
| `discord.webhooks.rate-limit-delay` | `2000` | Milliseconds between two audit messages to the same webhook. Messages that come sooner wait their turn |
| `discord.webhooks.max-retries` | `3` | How often a log message is sent again after a network error or a Discord outage. `0` turns retries off |
| `discord.webhooks.retry-delay` | `5000` | Milliseconds between those attempts |

An audit message is only dropped when the queue for its webhook is more than five minutes long. A
message Discord refuses, for example because the webhook was deleted, is not sent again.

`%timestamp%` is written in `localization.formats.datetime` and `localization.timezone`; see
[config.yml](../config/config-yml.md#localization).

Related: [Chat bridge](chat-bridge.md), [Rank and nickname sync](rank-sync.md)
