---
title: Player commands
order: 302
description: Joining, leaving, choosing a theme and a difficulty, the record, the boards and the language.
icon: user
---

Every command here is a subcommand of `/parkour` and needs `uxmparkour.play` (everyone), unless the table says
otherwise.

## Commands

### Running

| Command | What it does |
|---|---|
| `/parkour` | Joins the first open arena, the same as `/parkour join` with no arena |
| `/parkour join [arena]` | Joins that arena, or the first open arena in `arenas.conf` when none is named |
| `/parkour leave` | Ends your run and puts you back where you joined from, with your inventory |
| `/parkour restart` | Ends your run and starts a new one in the same arena at once, without giving the inventory back in between |

- `join` is refused while you are already in a run (`run.already-running`), for an arena that does not exist or is
  shut (`run.no-such-arena`), and when no arena is open at all (`run.no-arenas`).
- `leave` and `restart` answer `run.not-running` outside a run.
- A restart counts the old run on your record and the boards, like any other ending, and builds the new course from
  a new seed.
- The two session items do the same as `leave` and `restart`: see [Runs](../arenas/runs.md#the-session-items).

### Choices

| Command | What it does |
|---|---|
| `/parkour themes` | Opens the [theme picker](../menus/themes.md) |
| `/parkour theme <theme>` | Builds your runs from that theme, in every arena |
| `/parkour difficulty <difficulty>` | Runs you at that difficulty, in every arena |

A choice starts with your next run. An unknown name answers `theme.unknown` or `difficulty.unknown`. How a choice
is weighed against the arena's own is in [Concepts](../getting-started/concepts.md#what-a-player-chooses).

### Record and boards

| Command | What it does |
|---|---|
| `/parkour stats` | Your best run, your total jumps and your total runs |
| `/parkour top [board] [period]` | The first ten places of a board; `best` and `alltime` when left out |

`top` reads the same boards a hologram shows, from memory, so a run that just ended appears at the next board
refresh (`leaderboard.refresh-seconds`, 15 by default). It runs from the console too. An unknown board or period
answers `top.unknown`; an empty board, `top.empty`.

### Language

| Command | Permission | What it does |
|---|---|---|
| `/parkour lang` | `uxmparkour.language` | Says which language you read, and which this server has |
| `/parkour lang set <language>` | `uxmparkour.language` | Reads this plugin in that language, whatever your client is set to |
| `/parkour lang reset` | `uxmparkour.language` | Follows your client again |
| `/parkour lang set-player <player> <language>` | `uxmparkour.admin.language` | Sets the language another player reads |
| `/parkour lang reset-player <player>` | `uxmparkour.admin.language` | Clears another player's choice |

- `set` and `reset` are refused with an explanation when `language.force` names one language for everybody.
- A language is named by its tag (`tr`, `pt-BR`, `pt_BR`), in any case; `pt` alone works while only one Portuguese
  file exists. Suggestions list the languages in `messages/`.
- A player who is offline reads the new language when they next join.
- With uxmLang installed, the choice is shared with every UXM plugin. See [Languages](../config/languages.md).

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmparkour.play` | everyone | Every command on this page except the language ones |
| `uxmparkour.language` | everyone | `lang`, `lang set`, `lang reset` |
| `uxmparkour.admin.language` | op | `lang set-player`, `lang reset-player` |

Related: [Runs](../arenas/runs.md), [Theme picker](../menus/themes.md), [Placeholders](../placeholders/reference.md)
