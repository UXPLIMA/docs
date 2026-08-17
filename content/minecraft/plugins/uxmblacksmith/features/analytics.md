---
title: Analytics and the audit trail
order: 211
description: Every trade event recorded, queryable in chat, a menu, or a CSV export.
icon: chart-column
---

Every trade outcome is written to the database. Nothing is sampled and nothing expires on its own.

## The event types

| Event | When |
|---|---|
| `TRADE_STARTED` | Requirements taken, slot filled |
| `TRADE_COMPLETED` | Finished and rewarded |
| `TRADE_BURNED` | Failed its success roll |
| `TRADE_FAILED` | Could not complete (a missing hook, an invalid reward) |
| `TRADE_CANCELLED` | The player cancelled it |
| `ADMIN_CANCELLED` | Staff cancelled it |
| `ADMIN_FORCE_COMPLETED` | Staff completed it |
| `INSTANT_COMPLETED` | An `instantCraft` trade succeeded |
| `INSTANT_BURNED` | An `instantCraft` trade burned |
| `INSTANT_FAILED` | An `instantCraft` trade could not complete |
| `REFUND_PENDING` | A refund was owed but not delivered |
| `REFUND_DELIVERED` | The refund reached the player |
| `REFUND_FAILED` | The refund could not be delivered |

Each row records the event type, the player, the trade id, the slot, the acting staff member if any,
the refund mode, whether a full refund was even possible, and a timestamp.

## Daily rollups

Alongside the raw events, counts are aggregated per day per trade in `trade_daily_stats`: started,
completed, burned, cancelled and failed. That is what the summary commands read, so a `total` query
does not scan the event log.

Dates are **UTC**, so "today" rolls over at 00:00 UTC regardless of where the server is.

## In chat

```
/blacksmith admin analytics <today|week|month|total>
```

Prints the five counts for the period, then the top trades by volume with their own breakdown. Use
it to answer the two questions that actually matter: which recipes people use, and which ones burn
so often that nobody starts them twice.

## In a menu

```
/blacksmith admin analytics gui [period]
```

Defaults to `today`. The same figures with the period switchable in-place.

## One player's history

```
/blacksmith admin history <player> [limit]
```

Newest first. `limit` defaults to 10 and is capped at 50. Each line is the event type, the trade,
the slot, who did it, and the time, which is the answer to "my legendary sword disappeared".

## Export

```
/blacksmith admin export <csv|json> [today]
```

Writes today's events to a file in the plugin folder and prints the absolute path. CSV for a
spreadsheet, JSON for anything else.

The export runs off the main thread and reports back when it lands, so it does not stall the server
on a busy day.

## The pending queue

```
/blacksmith admin queue list [player]
/blacksmith admin queue gui
/blacksmith admin queue cancel <player> <slot> [refund|no-refund]
/blacksmith admin queue complete <player> <slot>
```

The live view of what is running, and the tools to intervene. `cancel` refunds by default; pass
`no-refund` to take the materials.

A refund the player cannot receive (offline, full inventory) becomes a row in `pending_refunds`
and is delivered on their next login. `REFUND_PENDING` then `REFUND_DELIVERED` in the event log is
the normal, healthy sequence, not an error.

<Callout type="warning" title="A command reward is not refundable">

`refundComplete` on an event records whether everything could be returned. A trade whose reward ran
commands, or whose requirement was consumed by a hook that has since been disabled, cannot be made
whole. Check that flag before telling a player they got everything back.

</Callout>

## Placeholders

```
%uxmblacksmith_analytics_today_started%
%uxmblacksmith_analytics_today_completed%
%uxmblacksmith_analytics_today_burned%
%uxmblacksmith_analytics_today_cancelled%
%uxmblacksmith_analytics_today_failed%
```

Server-wide, cached, safe on a scoreboard.

## Retention

Nothing prunes `trade_events` or `progression_events`. On a busy server they grow steadily. They are
indexed by player, trade and timestamp, so queries stay fast, but the file does not shrink: plan a
periodic archive if you run for years.
