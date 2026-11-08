---
title: webhooks.yml
order: 406
description: The Discord webhook's settings and the text posted for each build and market event.
icon: webhook
---

Off until you switch it on and paste a webhook URL. The settings and the texts sit together in this
one file, in the language set in `config.yml`. For setting the webhook up in Discord, see
[Discord notifications](../features/notifications.md).

```yaml
enabled: false
url: ''
username: 'uxmBuilders'

# Builds: START | COMPLETE | CANCEL
# Player market: LISTED | SOLD | REPORTED | REMOVED
events: [ START, COMPLETE, CANCEL, LISTED, SOLD, REPORTED, REMOVED ]

# Market reports go here instead, for a channel only staff can read.
# Empty: they go to the url above.
moderationUrl: ''

buildStarted: ':construction: %player.name% started **%package.name%** (#%job.id%, %job.total% blocks) at %location.world% %location.x%, %location.y%, %location.z%'
buildCompleted: ':white_check_mark: %player.name% finished **%package.name%** (#%job.id%, %job.total% blocks)'
buildCancelled: ':x: **%package.name%** of %player.name% (#%job.id%) was cancelled at %job.progress%%: %reason%, refund %economy.refund%'

marketListed: ':shopping_cart: %seller.name% listed **%listing.title%** (%listing.code%) for %listing.price%'
marketSold: ':moneybag: %buyer.name% bought **%listing.title%** (%listing.code%) from %seller.name% for %listing.price% (market fee %listing.fee%)'
marketReported: ':triangular_flag_on_post: %reporter.name% reported **%listing.title%** (%listing.code%) by %seller.name%: %report.reason%. %report.note% (report %report.id%, %listing.reports% open)'
marketRemoved: ':no_entry: **%listing.title%** (%listing.code%) by %seller.name% was removed by %moderator.name%: %reason%'
```

## Settings

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `false` | Post at all |
| `url` | empty | The Discord webhook URL; anything that is not `http://` or `https://` posts nothing and logs one warning |
| `username` | `uxmBuilders` | The name the posts appear under |
| `events` | all seven | Which of `START`, `COMPLETE`, `CANCEL`, `LISTED`, `SOLD`, `REPORTED` and `REMOVED` are posted |
| `moderationUrl` | empty | A second webhook that receives the `REPORTED` posts instead of `url`, so the players' channel never shows who reported whom. An unusable value is warned about and the post goes to `url` |

## Texts

| Key | Posted when |
|---|---|
| `buildStarted` | A build has really started: paid for and queued |
| `buildCompleted` | The last block is placed |
| `buildCancelled` | A build is cancelled, for any reason |
| `marketListed` | A design is put on sale |
| `marketSold` | A copy of a design is bought |
| `marketReported` | A player reports a listing |
| `marketRemoved` | A moderator takes a listing down, by hand or through a seller ban with `moderation.onSellerBan: REMOVE` |

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

The market texts take their own:

| Placeholder | Value |
|---|---|
| `%listing.title%`, `%listing.code%`, `%listing.price%`, `%seller.name%` | Every market text: the listing's title, code and price, and its seller |
| `%buyer.name%`, `%listing.fee%`, `%listing.net%` | `marketSold`: the buyer, the market's commission, and what the seller earns |
| `%reporter.name%`, `%report.reason%`, `%report.note%`, `%report.id%`, `%listing.reports%` | `marketReported`: who reported, why, their note, the start of the report's id, and the listing's open reports |
| `%moderator.name%`, `%reason%` | `marketRemoved`: the moderator (`Console` from the console) and the reason they gave |

Mentions are switched off on every post: nobody is pinged, not by a `<@id>` you write and not by a
package or listing named `@everyone`.

`market.yml` also has a `webhook` section with `events` and `moderationUrl`. It is not read: set both
here.

Related: [Discord notifications](../features/notifications.md), [Messages and menus](messages-and-menus.md), [market.yml](market-yml.md)
