---
title: Placeholders
order: 70
description: The five PlaceholderAPI placeholders, all about the running tournament.
icon: braces
---

With PlaceholderAPI installed, uxmSupplyRush registers the `supplyrush` expansion. Every
placeholder is about the event that is running now, and shows `-` when no event is running.

| Placeholder | Shows | Example |
|---|---|---|
| `%supplyrush_points%` | The player's points in this run | `2.440` |
| `%supplyrush_rank%` | The player's rank, or `Unranked` before their first deposit | `#3` |
| `%supplyrush_time_left%` | Time until the run ends, then `ending...` | `2d 4h 10m` |
| `%supplyrush_total%` | Items deposited by everyone in this run | `18.502` |
| `%supplyrush_next_start%` | The date of the next scheduled start, or `-` with the schedule off | `24.10.2026 18:00:00` |

Numbers follow `number-format` in [config.yml](config/config-yml.md), dates follow `date-format` in
`messages.yml`, and the rank and `ending...` texts come from the `format` section of
[messages.yml](config/languages.md#messagesyml).

These work in scoreboards, tab lists and holograms, and inside reward commands. Within the
plugin's own messages and announcements, use the [event file placeholders](config/event-files.md#announcements)
instead.

Related: [Ranking](features/ranking.md), [Automatic starts](features/auto-start.md)
