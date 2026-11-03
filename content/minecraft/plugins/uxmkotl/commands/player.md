---
title: Player commands
order: 302
description: Every /kotl subcommand, with its arguments, what it does and the permission it needs.
icon: user
---

Every player command is under `/kotl`, alias `/ladder`, and needs `uxmkotl.play`, which everyone has by default.

## Playing

| Command | Permission | What it does |
|---|---|---|
| `/kotl` | `uxmkotl.play` | Lists the arenas that are switched on, by id. Runs from the console |
| `/kotl join <arena>` | `uxmkotl.play` | Puts you in the arena and teleports you to its spawn. Player only |
| `/kotl leave` | `uxmkotl.play` | Takes you out of the arena you are in and teleports you to its exit. Player only |

`join` answers, instead of joining:

| Reply | When |
|---|---|
| `error.unknown-arena` | No arena has that id, or the arena still misses a piece |
| `error.arena-disabled` | The arena is switched off |
| `error.already-playing` | You are in an arena already; leave it first |

On a successful join, the `joined` [moment](../arenas/moments.md) runs: by default *You joined hill. Take the plate at
the top.* Your items, effects and game mode are handled as the arena says ([Arenas](../arenas/overview.md#what-entry-does)).

`leave` ends your reign if you hold the crown, runs the `dethroned` moment with the reign's length, then the `left`
moment, and gives back what the arena took. Outside an arena it answers `error.not-playing`. Walking out of the box
does the same as `leave`, except that you stay where you are instead of going to the exit.

## Records and boards

| Command | Permission | What it does |
|---|---|---|
| `/kotl stats` | `uxmkotl.play` | Opens [your record](../menus/stats.md) and writes it in one line: time held, longest reign, crowns and kills. Player only |
| `/kotl top` | `uxmkotl.play` | For a player, opens the [leaderboard window](../menus/top.md). From the console, writes the `held` board for all time |
| `/kotl top <board> [period]` | `uxmkotl.play` | Writes one board for one period in chat, up to `leaderboard.size` lines. The period defaults to `alltime` |

The boards are `held`, `longest`, `claims` and `kills`; the periods `alltime`, `monthly`, `weekly` and `daily`. A
time board writes its scores as times, such as `12m 4s`. An unknown board or period answers `top.unknown`; a board
with nobody on it answers `top.empty`.

```
/kotl top kills weekly
```

## Language

| Command | Permission | What it does |
|---|---|---|
| `/kotl lang` | `uxmkotl.language` | Says which language you read uxmKotl in, and which the server has. Player only |
| `/kotl lang set <language>` | `uxmkotl.language` | Reads uxmKotl in that language from now on, whatever your client is set to. Player only |
| `/kotl lang reset` | `uxmkotl.language` | Forgets your choice; you read your client's language again. Player only |
| `/kotl lang set-player <player> <language>` | `uxmkotl.admin.language` | Sets another player's language. A player who is offline gets it at their next join |
| `/kotl lang reset-player <player>` | `uxmkotl.admin.language` | Clears another player's choice |

While `language.force` names a language, `lang set` and `lang reset` are refused with `language.forced`; an operator's
`set-player` is kept and takes effect when the force is lifted. With uxmLang installed, the choice is uxmLang's and
is shared with every UXM plugin. See [Languages](../config/languages.md).

Related: [Overview](overview.md), [Operator commands](admin.md), [Permissions](../permissions/reference.md)
