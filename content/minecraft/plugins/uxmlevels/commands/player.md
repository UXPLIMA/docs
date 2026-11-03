---
title: Player commands
order: 302
description: Every /levels subcommand, with its arguments, what it does and the permission it needs.
icon: user
---

`/levels`, aliases `/lvl` and `/level`.

| Command | Permission | What it does |
|---|---|---|
| `/levels` | `uxmlevels.use` | Opens the [levels window](../menus/main.md). Player only |
| `/levels level [track]` | `uxmlevels.use` | Your level on that track, or on every track. Player only |
| `/levels level of <player> <track>` | `uxmlevels.others` | Another player's level on a track |
| `/levels top <track> [period]` | `uxmlevels.use` | The best players on a track, in chat |
| `/levels prestige <track>` | `uxmlevels.prestige` | Prestiges that track. Player only |
| `/levels lang` | `uxmlevels.language` | Says which language you read uxmLevels in, and which the server has. Player only |
| `/levels lang set <language>` | `uxmlevels.language` | Reads uxmLevels in that language from now on. Player only |
| `/levels lang reset` | `uxmlevels.language` | Forgets your choice; you read your client's language again. Player only |
| `/levels lang set-player <player> <language>` | `uxmlevels.admin.language` | Sets another player's language |
| `/levels lang reset-player <player>` | `uxmlevels.admin.language` | Clears another player's choice |

Tab completion offers the track ids, the periods and the players.

## level

```
/levels level
Global: level 12 of 100, 140 of 459.
Mining: level 3 of 100, 52 of 195.
Combat: level 1 of 100, 9 of 150.
/levels level mining
Mining: level 3 of 100, 52 of 195.
```

A finished track answers `levels.maximum-reached`: `You have finished Mining.` A server with no track answers
`levels.no-tracks`.

## level of

```
/levels level of Steve combat
Steve on Combat: level 7 of 100.
```

## top

`/levels top <track>` prints the all time board: `leaderboard.chat-lines` lines, 10 by default, each with the
place, the player, the level and the experience, then `top.unranked` when you are not on the board.

`/levels top <track> <period>` with `monthly`, `weekly` or `daily` prints the experience earned on that track in the
current period. A period not kept in `leaderboard.periods` answers `top.unknown`.

## prestige

`/levels prestige mining` resets the mining track when the player meets the rules on
[Prestige](../tracks/prestige.md), and answers `prestige.done` with the new count and multiplier. The window does the
same from its own tile.

Related: [Overview](overview.md), [Levels window](../menus/main.md), [Languages](../config/languages.md)
