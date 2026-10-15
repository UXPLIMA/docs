---
title: Discord notifications
order: 212
description: Posting build starts, completions and cancellations to a Discord webhook.
icon: bell
---

uxmBuilders can post a line to a Discord channel, or any service that accepts Discord's webhook format,
when a build starts, finishes or is cancelled.

## Setting it up

1. In Discord: channel settings, **Integrations**, **Webhooks**, **New Webhook**, copy its URL.
2. In `config.yml`:

```yaml
notifications:
  webhook:
    enabled: true
    url: 'https://discord.com/api/webhooks/...'
    username: 'uxmBuilders'
    events: [ START, COMPLETE, CANCEL ]
```

3. `/builders admin reload`.

## Events

| Event | Posted when | Message key |
|---|---|---|
| `START` | A build has really started, money taken and job queued | `webhookBuildStarted` |
| `COMPLETE` | The last block is placed | `webhookBuildCompleted` |
| `CANCEL` | A build is cancelled, for any reason | `webhookBuildCancelled` |

The text lives in `messages.yml`, in your language, with its colour tags stripped. Discord markdown
and emoji codes work:

```yaml
webhookBuildCompleted: ':white_check_mark: %player.name% finished **%package.name%** (#%job.id%)'
```

| Placeholder | Value |
|---|---|
| `%player.name%` | The owner |
| `%package.name%` | The package's display name |
| `%job.id%`, `%job.total%`, `%job.progress%` | The job |
| `%location.world%`, `%location.x%`, `%location.y%`, `%location.z%` | Where it is |
| `%economy.refund%`, `%reason%` | Cancellations only |

## Notes

- Posts go out one at a time on a background thread; a slow or unreachable webhook never touches the
  main thread.
- A rate limit from Discord is honoured once with its retry delay. A backlog of more than 100 posts is
  dropped with a warning rather than held in memory.
- Mentions are disabled: a player who names a package `@everyone` pings nobody.
- An empty or invalid URL logs one warning and posts nothing.

Related: [Messages and menus](../config/messages-and-menus.md)
