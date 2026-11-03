---
title: Player commands
order: 202
description: Every /pvp subcommand, with its arguments, what it does and the permission it needs.
icon: user
---

Every player command is under `/pvp`, alias `/fight`, and needs `uxmhubpvp.use`, which everyone has by default.

## Fighting

| Command | Permission | What it does |
|---|---|---|
| `/pvp` | `uxmhubpvp.use` | The same as clicking the toggle item: starts the warm-up in, starts the warm-up out, or stops a warm-up. Player only |

The warm-up in takes 3 seconds and the warm-up out 8, as shipped; the player must stay within 1.5 blocks of where they
pressed. See [Turning PvP on and off](../fighting/overview.md).

## Records and boards

| Command | Permission | What it does |
|---|---|---|
| `/pvp stats` | `uxmhubpvp.stats` | Your record: kills, deaths, ratio, streak and best streak |
| `/pvp stats <player>` | `uxmhubpvp.stats` | Another player's record. Runs from the console |
| `/pvp top [board] [period]` | `uxmhubpvp.top` | One board for one period, up to `leaderboard.size` lines. The board defaults to `kills`, the period to `alltime`. Runs from the console |

```
The record of Steve
Kills 42, deaths 17, ratio 2.47
Streak 3, best 9
```

A name the server has never seen answers `error.unknown-player`. A player with no fights reads all zeros. An unknown
board or period answers `top.unknown`; a board with nobody on it answers `top.empty`.

```
/pvp top streak weekly
```

## Language

| Command | Permission | What it does |
|---|---|---|
| `/pvp lang` | `uxmhubpvp.language` | Says which language you read uxmHubPvP in, and which the server has. Player only |
| `/pvp lang set <language>` | `uxmhubpvp.language` | Reads uxmHubPvP in that language from now on, whatever your client is set to. Player only |
| `/pvp lang reset` | `uxmhubpvp.language` | Forgets your choice; you read your client's language again. Player only |
| `/pvp lang set-player <player> <language>` | `uxmhubpvp.admin.language` | Sets another player's language. A player who is offline gets it at their next join |
| `/pvp lang reset-player <player>` | `uxmhubpvp.admin.language` | Clears another player's choice |

While `language.force` names a language, `lang set` and `lang reset` are refused with `language.forced`. With uxmLang
installed, the choice is uxmLang's and is shared with every UXM plugin. See [Languages](../config/languages.md).

Related: [Overview](overview.md), [Operator commands](admin.md), [Permissions](../permissions/reference.md)
