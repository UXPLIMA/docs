---
title: Leaderboards
order: 313
description: Boards per job and in total, the four periods, the cache, and where boards are shown.
icon: trophy
---

A board is a job by its id, or `total`. All time ranks by level: who is ahead now. A month, a week and a day rank by
what was earned in them: experience on a job's board, levels on the total.

```hocon
leaderboard {
  chat-lines   = 10
  empty-head   = "entity/player/wide/steve"
  periods      = [ alltime, monthly, weekly, daily ]
  time-zone    = ""
  week-starts  = monday
  keep-windows = 12
}
```

| Key | Meaning |
|---|---|
| `chat-lines` | How many places `/jobs top` prints |
| `empty-head` | What a head placeholder gives for an empty place: a texture path, a name or an id |
| `periods` | The periods kept; empty keeps all four |
| `time-zone` | The zone a day starts in (`Europe/Istanbul`); empty is the server's. Servers sharing a database write the same |
| `week-starts` | The day a week starts |
| `keep-windows` | How many ended windows of a period are kept, so last week's winner can be read |

A board reads `storage.leaderboard-size` (100) rows and is refreshed every `storage.leaderboard-refresh-seconds` (120).
A window or a placeholder never waits for the database.

## Where boards are shown

- `/jobs top [job] [period]` in chat.
- `/jobs board`, the [leaderboard window](../menus/top.md).
- Top [signs](signs.md).
- The [board placeholders](../placeholders/reference.md#boards) for holograms and scoreboards.

Related: [Placeholders](../placeholders/reference.md)
