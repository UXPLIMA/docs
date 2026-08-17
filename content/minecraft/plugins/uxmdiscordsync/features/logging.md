---
title: Logging
order: 38
description: Chat, join/leave and audit logs, each through its own webhook.
icon: scroll-text
---

Three logs, all in `logging.yml`, all posted through webhooks rather than the bot: chat, join/leave,
and audit.

Each section takes its own `webhook-url`, so each can go to a different channel with different
permissions, which is the point of separating them.

## Chat logging

```yaml
chat-logging:
  enabled: true
  webhook-url: "YOUR_WEBHOOK_URL_HERE"
  format: "**[%server_name%]** %player%: %message%"
  use-embed: false
```

Placeholders: `%player%`, `%player_displayname%`, `%message%`, `%server_name%`, `%world%`,
`%timestamp%`.

`use-embed: true` switches to an embed with the player's head as the author icon. Plain text is
denser and easier to search; embeds read better. For a log you actually scroll through looking for
something, plain text wins.

### Filtering

```yaml
filter:
  ignore-prefixes: ["/", "!", "#"]
  min-length: 1
  max-length: 500
  ignored-permission: ""
```

`ignore-prefixes` keeps commands out of the chat log, including any typed as `/msg`, which is why
`/` is there by default. Setting `ignored-permission` to something like
`uxmdiscordsync.chatlog.ignore` exempts staff from being logged.

`max-length: 500` truncates rather than dropping.

### Rate limiting

```yaml
rate-limit:
  enabled: true
  max-messages-per-minute: 30
```

Thirty is Discord's own webhook allowance. Raising it does not raise Discord's limit: it only means
messages get dropped by Discord instead of by the plugin.

## Join and leave

Two independent sections: `join-leave-logging.minecraft` for the game server, and
`join-leave-logging.discord` for guild membership. Each has its own webhook and its own join and
leave messages, in plain text or embed form.

Minecraft placeholders: `%player%`, `%player_displayname%`, `%player_uuid%`, `%server_name%`,
`%online_players%`, `%max_players%`.

Discord placeholders: `%user%`, `%user_tag%`, `%user_id%`, `%user_avatar_url%`, `%member_count%`.

Discord member logging requires the **Server Members** intent.

## Audit logging

```yaml
audit:
  enabled: true
  account-link:
    enabled: true
    webhook-url: "..."
  account-unlink:
    enabled: true
  boost-claim:
    enabled: true
  rank-sync:
    enabled: true
```

Four event types, each with its own webhook and its own embed. These are the records you go back to
when something is disputed: who linked to which Discord account and when, who claimed a boost
reward, what roles a sync actually changed.

Point them at a staff-only channel. `%player_uuid%` and `%discord_id%` in the default embeds are
what make an entry identifiable months later, and neither belongs in a public channel.

`rank-sync` fills `%minecraft_ranks%` and `%discord_roles%` with what the player had at the time,
which turns "the plugin removed my role" into a question with an answer.

## Retention

```yaml
cleanup:
  delete-old-logs-after-days: 30
```

in `advanced.yml`. Applies to audit records held in the database; the webhook posts in Discord stay
until you delete them. Set it to `0` to keep everything.

<Callout type="note" title="Webhook URLs are credentials">

A webhook URL is enough for anyone to post in that channel as the webhook, no bot, no token, no
membership. Keep `logging.yml` out of public repositories and out of pastes in support channels, and
delete and recreate the webhook if one leaks.

</Callout>
