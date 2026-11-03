---
title: Player commands
order: 202
description: Every /eggs subcommand, with its arguments, what it does and the permission it needs.
icon: user
---

Every player command is under `/eggs`, aliases `/easteregg` and `/eastereggs`, and needs `uxmeastereggs.use`, which
everyone has by default. Finding an egg needs no command and no node: a click does it.

## Hunting

| Command | Permission | What it does |
|---|---|---|
| `/eggs` | `uxmeastereggs.use` | Opens [your collection](../menus/collection.md). Player only |
| `/eggs radar` | `uxmeastereggs.use` | Switches the radar on or off, and runs `radar-on` or `radar-off`. Player only |

The radar starts in the state `radar.default-on` gives, off as shipped, and a flip lasts until the player leaves the
server. See [Radar and particles](../hunting/radar-and-particles.md).

## Boards

| Command | Permission | What it does |
|---|---|---|
| `/eggs top` | `uxmeastereggs.use` | For a player, opens the [leaderboard window](../menus/leaderboard.md). From the console, writes the `found` board for all time |
| `/eggs top <board> [period]` | `uxmeastereggs.use` | Writes one board for one period in chat, up to `leaderboard.size` lines. The period defaults to `alltime` |

```
/eggs top found weekly
```

An unknown board or period answers `top.unknown`; a board with nobody on it answers `top.empty`.

## Language

| Command | Permission | What it does |
|---|---|---|
| `/eggs lang` | `uxmeastereggs.language` | Says which language you read uxmEasterEggs in, and which the server has. Player only |
| `/eggs lang set <language>` | `uxmeastereggs.language` | Reads uxmEasterEggs in that language from now on. Player only |
| `/eggs lang reset` | `uxmeastereggs.language` | Forgets your choice; you read your client's language again. Player only |
| `/eggs lang set-player <player> <language>` | `uxmeastereggs.admin.language` | Sets another player's language |
| `/eggs lang reset-player <player>` | `uxmeastereggs.admin.language` | Clears another player's choice |

While `language.force` names a language, `lang set` and `lang reset` are refused. With uxmLang installed, the choice is
uxmLang's. See [Languages](../config/languages.md).

Related: [Overview](overview.md), [Operator commands](admin.md), [Permissions](../permissions/reference.md)
