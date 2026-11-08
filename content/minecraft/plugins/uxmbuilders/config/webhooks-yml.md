---
title: webhooks.yml
order: 406
description: The Discord webhook's settings and the text posted for each build event.
icon: webhook
---

Off until you switch it on and paste a webhook URL. The settings and the texts sit together in this
one file, in the language set in `config.yml`. For setting the webhook up in Discord, see
[Discord notifications](../features/notifications.md).

```yaml
enabled: false
url: ''
username: 'uxmBuilders'

# START | COMPLETE | CANCEL
events: [ START, COMPLETE, CANCEL ]

buildStarted: ':construction: %player.name% started **%package.name%** (#%job.id%, %job.total% blocks) at %location.world% %location.x%, %location.y%, %location.z%'
buildCompleted: ':white_check_mark: %player.name% finished **%package.name%** (#%job.id%, %job.total% blocks)'
buildCancelled: ':x: **%package.name%** of %player.name% (#%job.id%) was cancelled at %job.progress%%: %reason%, refund %economy.refund%'
```

## Settings

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `false` | Post at all |
| `url` | empty | The Discord webhook URL; anything that is not `http://` or `https://` posts nothing and logs one warning |
| `username` | `uxmBuilders` | The name the posts appear under |
| `events` | all three | Which of `START`, `COMPLETE` and `CANCEL` are posted |

## Texts

| Key | Posted when |
|---|---|
| `buildStarted` | A build has really started: paid for and queued |
| `buildCompleted` | The last block is placed |
| `buildCancelled` | A build is cancelled, for any reason |

The texts are plain Discord messages, up to 2,000 characters. MiniMessage tags and `&` colour codes
are stripped, while Discord's own forms stay: markdown, emoji codes, `<@id>` mentions, `<#id>` channels,
`<t:…>` timestamps and links in angle brackets.

| Placeholder | Value |
|---|---|
| `%player.name%` | The owner |
| `%package.name%`, `%package.id%` | The package's display name, and its id |
| `%job.id%`, `%job.total%`, `%job.placed%`, `%job.progress%` | The job: id, blocks in total, blocks placed, percent done |
| `%location.world%`, `%location.x%`, `%location.y%`, `%location.z%` | Where it is |
| `%economy.refund%`, `%reason%` | Cancellations only; the reason is the `cancelReason…` label from `messages.yml` |

Mentions are switched off on every post: nobody is pinged, not by a `<@id>` you write and not by a
package named `@everyone`.

Related: [Discord notifications](../features/notifications.md), [Messages and menus](messages-and-menus.md)
