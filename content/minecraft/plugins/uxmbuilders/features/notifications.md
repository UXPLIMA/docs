---
title: Discord notifications
order: 312
description: Posting build and player market events to a Discord webhook.
icon: bell
---

uxmBuilders can post a line to a Discord channel, or any service that accepts Discord's webhook format,
when a build starts, finishes or is cancelled, and when the [player market](market/index.md) lists, sells,
reports or removes a design. The settings and the texts are in [webhooks.yml](../config/webhooks-yml.md).

## Setting it up

1. In Discord: channel settings, **Integrations**, **Webhooks**, **New Webhook**, copy its URL.
2. In `plugins/uxmBuilders/webhooks.yml`:

```yaml
enabled: true
url: 'https://discord.com/api/webhooks/...'
username: 'uxmBuilders'
events: [ START, COMPLETE, CANCEL, LISTED, SOLD, REPORTED, REMOVED ]
moderationUrl: ''
```

3. `/builders admin reload`.

## Events

| Event | Posted when | Text |
|---|---|---|
| `START` | A build has really started: money taken and job queued | `buildStarted` |
| `COMPLETE` | The last block is placed | `buildCompleted` |
| `CANCEL` | A build is cancelled, for any reason | `buildCancelled` |
| `LISTED` | A design is put on sale | `marketListed` |
| `SOLD` | A copy of a design is bought | `marketSold` |
| `REPORTED` | A player reports a listing; to `moderationUrl` when one is set | `marketReported` |
| `REMOVED` | A moderator takes a listing down | `marketRemoved` |

Builds started by staff with `/builders admin build` are posted too. Point `moderationUrl` at a
staff-only channel to keep reports, and who made them, out of the players' channel. The texts are in
your language, with colour tags stripped; Discord markdown and emoji codes work:

```yaml
buildCompleted: ':white_check_mark: %player.name% finished **%package.name%** (#%job.id%)'
```

Every placeholder is listed on the [webhooks.yml](../config/webhooks-yml.md) page.

## Notes

- Posts go out one at a time on a background thread; a slow or unreachable webhook never touches the
  main thread or a build.
- A rate limit from Discord is waited out once, for as long as Discord asks, up to a minute. A longer
  wait drops the posts until it is over, with one warning. A backlog of more than 100 posts is dropped
  with a warning rather than held in memory.
- Posts still queued when the server stops get up to three seconds to go out.
- Mentions are disabled: a player who names a package `@everyone` pings nobody.
- An empty or invalid URL logs one warning and posts nothing.

Related: [webhooks.yml](../config/webhooks-yml.md), [Messages and menus](../config/messages-and-menus.md)
