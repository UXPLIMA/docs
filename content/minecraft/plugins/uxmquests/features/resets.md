---
title: Resets
order: 402
description: When a board is thrown away and a new one drawn, and why the timezone is in the file.
icon: clock
---

| `reset.type` | A new board is drawn |
|---|---|
| `wall-clock` | At `at` (`"04:00"`), every day, in `zone`. Everybody at once |
| `rolling` | `every` (default `24h`) after the player's own board was handed out |
| `cron` | By the five field expression in `cron`, in `zone` |
| `never` | Never. What a catalogue writes |

```hocon
reset { type = cron, cron = "0 4 * * 1", zone = "Europe/Istanbul" }   # every Monday at 04:00
```

The timezone is always read from the file, never from the machine, so moving the server to another host does not move
the reset. The board shows how long until it is drawn again. `/uxmquests reset board <player> <group>` throws one
player's board away now.

Related: [Groups and boards](groups-and-boards.md)
