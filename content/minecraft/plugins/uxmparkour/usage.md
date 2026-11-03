---
title: "Guide"
order: 10
description: "How uxmParkour works, how to set it up and what each file holds."
---

uxmParkour builds an endless parkour course in front of a player as they jump. Every jump is generated from the player's difficulty and theme, the blocks behind them vanish, and the run ends when they fall. Runs pay milestones and feed leaderboards for all time, the month, the week and the day.

## Install

1. Put the jar in `plugins/`.
2. Start the server once. The plugin writes its files into `plugins/uxmParkour/`.
3. Make an arena (see below) and open it with `/uxmparkour enable <name>`.

PlaceholderAPI, Vault and the other economies are optional. With none of them installed the shipped milestones still pay experience.

## Files

| File | What it holds |
|---|---|
| `config.conf` | storage, run rules, leaderboards, difficulties, themes, effects, milestones, session items, feedback |
| `arenas.conf` | the arenas, written by the admin commands; it may be edited by hand |
| `menus/arenas.conf`, `menus/arena-editor.conf`, `menus/themes.conf` | the arena list, the arena editor and the theme picker |
| `commands.conf` | the name, aliases and on or off switch of every command |
| `messages/messages_<language>.conf` | the words players read, one file per language |
| `theme.conf` | the colours of the messages |

## Arenas

An arena is a box the course may never leave, a start block and a heading. Make one in game:

```
/uxmparkour create hub
/uxmparkour set hub start here
/uxmparkour set hub corner1 -64 70 -64
/uxmparkour set hub corner2 320 200 64
/uxmparkour set hub heading EAST
/uxmparkour enable hub
```

`create <name> [world]` writes the arena empty and shut and opens its editor. `set <name> <piece> <value>` sets one piece:

| Piece | Value |
|---|---|
| `start` | `x y z`, or `here` for the block under you |
| `corner1`, `corner2` | `x y z` or `here`; any two opposite corners of the box, in any order |
| `heading` | `NORTH`, `EAST`, `SOUTH` or `WEST`; the course runs that way |
| `world` | the world name |
| `difficulty` | a difficulty id from `config.conf` |
| `theme` | a theme id from `config.conf` |

The start block and the two blocks above it must be inside the box. An arena opens only when every piece is set and the pieces are legal; `show <name>` prints every piece, set or not. `disable` shuts it while you change it.

In `arenas.conf` the same arena reads:

```hocon
arenas {
  hub {
    world      = "world"
    heading    = "EAST"
    difficulty = "normal"
    theme      = "stone"
    enabled    = true
    start      { x = 0,    y = 100, z = 0 }
    corner-one { x = -64,  y = 70,  z = -64 }
    corner-two { x = 320,  y = 200, z = 64 }
  }
}
```

An arena that cannot be read is skipped with its name in the console; the others still load.

## Runs

A player starts with `/parkour` or `/parkour join [arena]`. Without a name they join the first open arena. Their inventory is stored for the run and given back when it ends, and they carry two session items (restart and leave) set under `session-items`, each with an `enabled` switch, a `slot` (0 to 8 is the hotbar) and a `material`.

| `run` key | Meaning |
|---|---|
| `blocks-ahead` | how many blocks stand in front of the player |
| `blocks-behind` | how many stay behind before they vanish; `0` removes a block as soon as the player leaves it |
| `max-fall` | how far below the last block they stood on a player may drop before the run ends |
| `default-difficulty`, `default-theme` | what a player gets when they chose none and the arena names one that no longer exists |
| `protect-arena` | stop anyone digging or building inside an arena with a run in it; `uxmparkour.admin.build` is never stopped |
| `announce-seed` | tell the player the seed their course came from |

When the box has no room left for another jump, the run ends on its own with the `exhausted` effect.

## Difficulties

Each block under `difficulties` limits the jumps a course may use, measured from the block the player is on:

| Key | Meaning |
|---|---|
| `min-forward`, `max-forward` | blocks along the heading; at least 1 |
| `max-sideways` | blocks either side of that line |
| `min-rise`, `max-rise` | the height change; negative is down |

Only jumps a player can make are kept: up to four blocks flat, three when rising one, five when dropping one, six when dropping two or more, never a rise of two, and never a block right beside the last. A range that leaves no jump possible is refused on reload with the reason. Players choose with `/parkour difficulty <id>`.

## Themes

Each block under `themes` has an `icon` for the picker and a list of `materials`; every block of a run is one of them at random. Players choose with `/parkour theme <id>` or from `/parkour themes`.

## Milestones and money

A milestone fires when a run reaches its jump count exactly:

```hocon
milestones {
  "25"  { money = ["exp 25"],  broadcast = false }
  "100" { money = ["exp 150", "vault 500"], broadcast = true }
}
```

Each money line is an economy and an amount: `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>` or `treasury[:<currency>]`. A line naming an economy the server does not run pays nothing and is logged once at start; the milestone still fires. `broadcast = true` also runs the `broadcast` effect. `economy.treasury-wait-ms` is how long one Treasury payment may take.

## Effects and feedback

`effects` holds a list of actions for five moments: `fell`, `exhausted`, `milestone`, `broadcast` and `personal-best`. Each line is `[verb] payload`, run in order, and an empty list silences the moment.

| Verb | Payload |
|---|---|
| `[message]` | a line to the player |
| `[broadcast]` | a line to everybody |
| `[title]` | a title |
| `[subtitle]` | `<title> \| <subtitle> <in> <stay> <out>` |
| `[actionbar]` | a line over the hotbar |
| `[sound]` | `<name> <volume> <pitch>` |
| `[particle]` | `<name> <count> <spread> [data]` |
| `[console]` | a command run by the server; `%player_name%` is the player |
| `[player]` | a command run by the player |
| `[close]` | close the open menu |

Text that starts with `@` names a line of the messages file. A verb may carry `{delay=40}` or `{chance=0.2}` before its payload.

`feedback` sets the fixed sound and particle for every jump, every new block and every removed block. Each has `enabled`; `pitch-step` raises the jump sound per jump up to `max-pitch`, so a streak can be heard.

## Leaderboards

There are three boards, `best` (the longest run), `jumps` and `runs`, each kept per period: `alltime`, `monthly`, `weekly`, `daily`.

| `leaderboard` key | Meaning |
|---|---|
| `size` | how many places each board keeps |
| `refresh-seconds` | how often the boards are read from the database; a change needs a restart |
| `empty-head` | what `%uxmparkour_top_<board>_<period>_<n>_head%` gives for a place nobody holds |
| `periods` | which periods are kept; empty keeps all four |
| `time-zone` | the zone a day starts in, such as `"Europe/Istanbul"`; empty is the server's own |
| `week-starts` | the first day of a week |
| `keep-windows` | how many ended windows of a period are kept before they are deleted |

Servers that share one database should use the same `time-zone`. A finished run shows on the board at the next read.

## Storage

`storage.type` is `sqlite` (the default, in `data/parkour.db`), `mysql`, `mariadb` or `postgresql`. For a database server fill `host`, `port` (`0` is the default port), `database`, `username` and `password`. `table-prefix` lets several plugins share one database, `pool-size` caps the connections, and `sync-seconds` is how often a server reads what other servers changed for a player who is here (`0` turns it off).

`local-only = true` keeps one server of a network on its own SQLite file. To move from SQLite to a database server, set the new type with `copy-from-file = true` and restart once; rows are copied only into empty tables.

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/parkour` | `uxmparkour.play` | start a run in the first open arena |
| `/parkour join [arena]` | `uxmparkour.play` | start a run |
| `/parkour leave` | `uxmparkour.play` | stop the run |
| `/parkour restart` | `uxmparkour.play` | start the run again from the first jump |
| `/parkour themes` | `uxmparkour.play` | the theme picker |
| `/parkour theme <theme>` | `uxmparkour.play` | choose the theme of your runs |
| `/parkour difficulty <difficulty>` | `uxmparkour.play` | choose the difficulty of your runs |
| `/parkour stats` | `uxmparkour.play` | your best run, jumps and runs |
| `/parkour top [board] [period]` | `uxmparkour.play` | a leaderboard; `best` and `alltime` by default |
| `/parkour lang`, `lang set <language>`, `lang reset` | `uxmparkour.language` | your own language |
| `/parkour lang set-player <player> <language>`, `lang reset-player <player>` | `uxmparkour.admin.language` | another player's language |
| `/uxmparkour list` | `uxmparkour.admin.list` | every arena |
| `/uxmparkour show <name>` | `uxmparkour.admin.list` | every piece of one arena |
| `/uxmparkour create <name> [world]` | `uxmparkour.admin.create` | make an arena |
| `/uxmparkour set <name> <piece> <value>` | `uxmparkour.admin.edit` | set one piece |
| `/uxmparkour edit [name]` | `uxmparkour.admin.edit` | the editor menu, or the arena list |
| `/uxmparkour enable <name>`, `disable <name>` | `uxmparkour.admin.edit` | open or shut an arena |
| `/uxmparkour delete <name>` | `uxmparkour.admin.delete` | delete an arena |
| `/uxmparkour tp <name>` | `uxmparkour.admin.tp` | stand at the start block |
| `/uxmparkour reset <player>` | `uxmparkour.admin.reset` | clear a runner's totals and board standings |
| `/uxmparkour reload` | `uxmparkour.admin.reload` | read every file again |
| `/uxmparkour version` | `uxmparkour.admin.version` | show the running version |
| `/uxmparkour doctor` | `uxmparkour.admin.doctor` | report the plugin's health |

The names and aliases come from `commands.conf`. The [reference](reference.md) lists the default aliases and every permission.

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmparkour_current%` | the jumps in the run the player is on, or 0 |
| `%uxmparkour_arena%` | the arena of that run, or empty |
| `%uxmparkour_best%` | the player's longest run |
| `%uxmparkour_total_jumps%` | the player's jumps over every run |
| `%uxmparkour_total_runs%` | the player's runs |
| `%uxmparkour_theme%`, `%uxmparkour_difficulty%` | the player's choice, or empty |
| `%uxmparkour_head%` | the player's head, for a `<head:...>` tag |
| `%uxmparkour_top_<board>_<period>_<n>_name%` | the name at place n |
| `%uxmparkour_top_<board>_<period>_<n>_value%` | the score at place n |
| `%uxmparkour_top_<board>_<period>_<n>_head%` | the head at place n, or `empty-head` |
| `%uxmparkour_rank_<board>_<period>%` | the player's own place |
| `%uxmparkour_value_<board>_<period>%` | the player's own score |
| `%uxmparkour_resets_<period>%` | the time until that period starts again |

`<board>` is `best`, `jumps` or `runs`, and `<period>` is `alltime`, `monthly`, `weekly` or `daily`. For a player who is not online the personal totals answer 0; the `top` family names players who are away. A place nobody holds answers the messages file's word for nobody or none.

## Troubleshooting

- **An arena will not open.** `/uxmparkour show <name>` prints every piece, set or not; the start and the two blocks above it must sit inside the box.
- **A difficulty is refused on reload.** Its range allows no jump a player can make; widen `max-forward` or lower `min-rise`.
- **Runs end at once with "exhausted".** The box is too small for the difficulty; make it larger.
- **A milestone pays nothing.** The money line names an economy this server does not run; the console said so at start.
