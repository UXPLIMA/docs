---
title: Parkour
order: 311
description: An island that is a course its team builds and anybody runs.
icon: footprints
---

A Parkour island is a course. Its team builds the course; anybody runs it. A run starts on a plate over the start
block, is saved at every checkpoint plate, and ends on a plate over the finish block. The best time of every runner on
every course is kept.

| During a run | What happens |
|---|---|
| A checkpoint | Saves the run; the time is compared with your best there |
| A fall | Puts you back on the last checkpoint |
| Flying, a teleport off the path, or a speed nobody runs | Ends the run |
| Too long | Ends the run |
| A ghost | An item only you see retraces the course record, or your own best, as `ghost` in the file chooses |

What a player carries is kept aside on a course and handed back when they leave. `uxmskyblock.parkour.keepmode` keeps
a player's game mode, and `uxmskyblock.parkour.checks.bypass` runs a course without its checks.

## Commands

| Command | What it does |
|---|---|
| `/is course [season [number]]` | The best times on the course you stand on, of all time or of a season |
| `/is courses` | Opens [every course](../menus/game-modes.md#parkour-courses) |
| `/is coursetag difficulty \| category [name]` | Tags the course you stand on; the course's team only |
| `/is courserate <stars>` | Rates it once you have finished it |

Difficulties ship as easy, medium, hard and expert; categories as classic, speed, maze, dropper and puzzle. Plates,
checks, ghosts, ratings and season times are in [parkour.conf](../config/parkour-conf.md). Staff verb:
`/isadmin parkour resettimes`.
