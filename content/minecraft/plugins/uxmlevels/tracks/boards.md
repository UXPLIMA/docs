---
title: Boards
order: 307
description: The boards of every track by all time, month, week and day, how often they are read, and where they show.
icon: trophy
---

Every track is a board, by its id. Each is kept for the periods in `leaderboard.periods`:

| Period | Ranks by | Starts again |
|---|---|---|
| `alltime` | Experience on the track, shown as the level | Never |
| `monthly` | Experience earned on the track this month | On the first of the month |
| `weekly` | Experience earned this week | On `week-starts` |
| `daily` | Experience earned today | At midnight |

## The keys

| Key | Default | What it does |
|---|---|---|
| `size` | `100` | Places a board holds. A player below has no position |
| `refresh-seconds` | `120` | How often the boards are read from the database |
| `chat-lines` | `10` | Places `/levels top` prints |
| `empty-head` | `"entity/player/wide/steve"` | The head of an empty place in `%uxmlevels_top_..._head%` |
| `periods` | `[ alltime, monthly, weekly, daily ]` | The periods kept; empty keeps all four |
| `time-zone` | `""` | The zone a day starts in, such as `"Europe/Istanbul"`; empty is the server's |
| `week-starts` | `monday` | The first day of a week |
| `keep-windows` | `12` | How many past months, weeks or days are kept, for a look back |

Every key but `chat-lines` and `empty-head` is read at start.

## Where they show

| Where | How |
|---|---|
| Chat | `/levels top <track> [period]` |
| A window | Right click a track in `/levels`: the [top players](../menus/top.md), all time |
| Placeholders | `%uxmlevels_top_<track>_<period>_<n>_name%` and the rest, see [Placeholders](../placeholders/reference.md#boards) |

A gain reaches the period boards as it happens and shows at the next refresh. `/uxmlevels reset` takes a player off
every board; `/uxmlevels set` and `take` change the all time board only, through the experience.

Related: [Placeholders](../placeholders/reference.md), [config.conf](../config/config-conf.md#leaderboard)
