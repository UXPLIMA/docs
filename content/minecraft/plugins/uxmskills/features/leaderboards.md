---
title: Leaderboards
order: 409
description: Boards per skill, total and average, the four periods, and hiding.
icon: trophy
---

A board is a skill by its id, `total` or `average`. All time is who is ahead now, by level. Monthly, weekly and daily
are what was earned in the period: experience on a skill's board, levels on total and average.

| Key in `leaderboard` | Default | Meaning |
|---|---|---|
| `size` | 100 | Places the snapshot holds; below it a player has no position |
| `chat-lines` | 10 | How many `/skills top` prints |
| `refresh-seconds` | 120 | How often the snapshot is rebuilt |
| `empty-head` | `entity/player/wide/steve` | What a place nobody holds draws |
| `periods` | `alltime`, `monthly`, `weekly`, `daily` | The periods kept |
| `time-zone` | `""`, the server's | Where a day starts |
| `week-starts` | `monday` | |
| `keep-windows` | 12 | Ended windows kept, so last week's winner can be read |

`uxmskills.hidden` keeps an account off every board. `/skills top`, `total`, `average` and `rank` read them in chat; the
[top window](../menus/skills.md#top) and the [placeholders](../placeholders/reference.md#boards) read the same.

Related: [Placeholders](../placeholders/reference.md)
