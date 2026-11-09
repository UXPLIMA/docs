---
title: Placeholders
order: 380
description: The PlaceholderAPI expansion for scoreboards, tab lists and holograms.
icon: braces
---

Identifier `uxmbuilders`, registered automatically when PlaceholderAPI is installed.

A player can run more than one build. The build placeholders report the **first** active one, in the
order the builds were queued, which is what a scoreboard line wants. Every value is read from memory,
never from the database, so the placeholders are safe to refresh often and from any thread.

## Builds

| Placeholder | Returns | With no build |
|---|---|---|
| `%uxmbuilders_active%` | Builds running, queued or paused | `0` |
| `%uxmbuilders_current_package%` | The package's display name, without colours | `-` |
| `%uxmbuilders_state%` | `BUILDING`, `QUEUED` or `PAUSED` | `-` |
| `%uxmbuilders_progress%` | Progress with a percent sign, `42%` | `0` |
| `%uxmbuilders_progress_raw%` | The number alone, `42` | `0` |
| `%uxmbuilders_progress_bar%` | A ten-segment bar | an empty bar |
| `%uxmbuilders_placed%` | Blocks placed | `0` |
| `%uxmbuilders_total%` | Blocks in the build | `0` |
| `%uxmbuilders_eta%` | Time left, `3m 20s` or `1h 5m` | `-` |
| `%uxmbuilders_queue_position%` | Builds ahead of it in the server queue | `0` |

`queue_position` is `0` while the build is running and when it is next in line; read `state` to tell
the two apart. A paused build counts the queued builds ahead of it in the server queue.

## Builder

| Placeholder | Returns |
|---|---|
| `%uxmbuilders_builder%` | The selected builder's name, without colours |
| `%uxmbuilders_builder_level%` | Its level |
| `%uxmbuilders_builder_speed%` | Its blocks per second at that level |

The builder placeholders return `-` for an offline player, whose profile is not in memory. An unknown
name after `uxmbuilders_` is left as written.

## Market

The [player market](features/market/index.md)'s figures for the player.

| Placeholder | Returns |
|---|---|
| `%uxmbuilders_market_listings%` | Their listings still on the market: for sale, paused, sold out or under review |
| `%uxmbuilders_market_sold%` | Copies sold of those listings |
| `%uxmbuilders_market_earnings%` | Everything the market credited them, paid, owed or on hold, less what refunds took back |
| `%uxmbuilders_market_pending%` | What the market still owes them; earnings on hold are not counted, and a debt shows `0` |
| `%uxmbuilders_market_designs%` | Designs they bought and still hold; refunded and revoked ones are left out |

Withdrawn and removed listings, and the copies they sold, are not counted. Amounts are plain numbers,
without the currency symbol or thousands marks: `1250` or `1250.5`.

The figures are kept in memory for the players online on this server. They are worked out when a player
joins, after the market's news and payout, then again every minute, and dropped when the player leaves,
so a scoreboard never waits for the database. A sale or a payout therefore shows within a minute. Every
market placeholder returns `-` until the figures are known: for a player who is not online on this
server, and for the first seconds after joining.

Related: [Messages and menus](config/messages-and-menus.md), [Player market](features/market/index.md)
