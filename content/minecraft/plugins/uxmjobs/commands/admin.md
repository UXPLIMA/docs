---
title: Admin commands
order: 303
description: Every /uxmjobs subcommand, with its arguments, node and what it does.
icon: shield
---

Every node here is operators only by default and hides its subcommand from anyone who lacks it.

| Command | Node | What it does |
|---|---|---|
| `/uxmjobs` | `uxmjobs.admin.version` | Says to add `help`, which lists what you may use |
| `/uxmjobs version` | `uxmjobs.admin.version` | The version this server runs |
| `/uxmjobs doctor` | `uxmjobs.admin.doctor` | One line per check; see [Installation](../getting-started/installation.md#checking-the-install) |
| `/uxmjobs reload` | `uxmjobs.admin.reload` | Reads every file again |
| `/uxmjobs editor` | `uxmjobs.admin.editor` | The [in-game editor](../features/editor.md) |
| `/uxmjobs experience <player> <job> <amount>` | `uxmjobs.admin.experience` | Adds job experience; a negative amount takes it |
| `/uxmjobs setlevel <player> <job> <level>` | `uxmjobs.admin.setlevel` | Puts a player at an exact level |
| `/uxmjobs points <player> <amount>` | `uxmjobs.admin.points` | Adds points; a negative amount takes them |
| `/uxmjobs reset <player>` | `uxmjobs.admin.reset` | Takes a player out of every job and off every board |
| `/uxmjobs boost start <multiplier> <minutes>` | `uxmjobs.admin.boost` | A server-wide boost that survives a restart |
| `/uxmjobs boost stop` | `uxmjobs.admin.boost` | Ends that boost |
| `/uxmjobs preview <job>` | `uxmjobs.admin.preview` | What every row pays at levels 1, 10, 50 and 100 |
| `/uxmjobs report` | `uxmjobs.admin.report` | Money created in the window, per job |
| `/uxmjobs report menu` | `uxmjobs.admin.report` | The same as a [window](../menus/report.md) |
| `/uxmjobs alerts` | `uxmjobs.admin.report` | Players whose income stands out |
| `/uxmjobs prune` | `uxmjobs.admin.prune` | Drops every row past its retention now |
| `/uxmjobs inspect` | `uxmjobs.admin.inspect` | What the block or creature you look at is worth |
| `/uxmjobs sign update` | `uxmjobs.admin.sign` | Writes every job sign again |
| `/uxmjobs zone wand` | `uxmjobs.admin.zone` | Gives the zone wand |
| `/uxmjobs zone define <zone>` | `uxmjobs.admin.zone` | Makes a zone of the marked box |
| `/uxmjobs zone name` | `uxmjobs.admin.zone` | The same, asking for the name in a screen |
| `/uxmjobs zone delete <zone>` | `uxmjobs.admin.zone` | Takes a zone away, and the blocks it still owed |
| `/uxmjobs zone list` | `uxmjobs.admin.zone` | Every zone, its size and multipliers |
| `/uxmjobs zone requirement <zone> <line>` | `uxmjobs.admin.zone` | Adds a requirement line to a zone |
| `/uxmjobs zone requirements clear <zone>` | `uxmjobs.admin.zone` | Takes every requirement off |
| `/uxmjobs zone regenerate` | `uxmjobs.admin.zone` | Puts the blocks that are due back now |
| `/uxmjobs lang set-player <player> <language>` | `uxmjobs.admin.language` | Sets another player's language |
| `/uxmjobs lang reset-player <player>` | `uxmjobs.admin.language` | Clears another player's choice |

## reload

Reads `config.conf`, `protections.conf`, `economy.conf`, `display.conf`, the jobs and `titles.conf`, the contracts,
the shop, the zones, `boosters.conf` and `schedule.conf`, the windows, the messages and the theme, and writes every sign
again. A file that is wrong refuses the reload and the server keeps the last good snapshot. `storage`,
`editor.enabled` and `commands.conf` need a restart.

## boost start

`/uxmjobs boost start 2 60` doubles every wage, in every currency, for an hour, and broadcasts it. The boost is kept in
the database, so a restart does not end it. A multiplier of 0 or more is accepted.

## preview

```
Preview MINER
mine:stone : 0.36 / 0.45 / 0.85 / 1.35
mine:diamond_ore : 22.3 / 25 / 37 / 43
BOUND mine:diamond_ore is held by its bound at level 100.
```

Each row at levels 1, 10, 50 and 100, with a line wherever the job's `bounds` hold it.

## inspect

Look at a block or a creature and run it: it says what that target is worth in each job that prices it, written as
the job file names it.

## reset

Every job they hold, every job they left, their points and every standing on every board, in every window, go, on
every server of the database. They can join a job again from the start.

Related: [Report and tools](../features/report-and-tools.md), [Zones](../features/zones.md)
