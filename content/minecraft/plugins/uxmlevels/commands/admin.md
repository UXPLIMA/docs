---
title: Operator commands
order: 203
description: Every /uxmlevels subcommand, with its arguments, what it does and the permission it needs.
icon: shield
---

Every operator command is under `/uxmlevels`, alias `/levelsadmin`.

| Command | Permission | What it does |
|---|---|---|
| `/uxmlevels give <player> <track> <amount>` | `uxmlevels.admin.give` | Adds experience, exactly: no multiplier and no hourly cap |
| `/uxmlevels take <player> <track> <amount>` | `uxmlevels.admin.take` | Takes experience away, stopping at zero |
| `/uxmlevels set <player> <track> <level>` | `uxmlevels.admin.set` | Puts the player at the start of that level |
| `/uxmlevels reset <player>` | `uxmlevels.admin.reset` | Starts the player again on every track and takes them off every board |
| `/uxmlevels boost <player> <multiplier> <duration>` | `uxmlevels.admin.boost` | Starts a timed multiplier, such as `2 30m` |
| `/uxmlevels unboost <player>` | `uxmlevels.admin.boost` | Stops the player's boost |
| `/uxmlevels reload` | `uxmlevels.admin.reload` | Reads `config.conf`, the tracks, the events, the windows, the messages and the theme again |
| `/uxmlevels doctor` | `uxmlevels.admin.doctor` | Checks the storage, the windows and PlaceholderAPI |
| `/uxmlevels version` | `uxmlevels.admin.version` | Prints the version this server runs |
| `/uxmlevels` | `uxmlevels.admin.version` | Points to `/uxmlevels help` |

The language of another player is set under `/levels`: see [Player commands](player.md).

## give

```
/uxmlevels give Steve mining 500
You gave 500 to Steve on Mining.
```

The amount is added as written. A level up from `give` sends the player their level up line and runs every
automatic reward of the levels crossed, as an earned level up does. The amount reaches the period boards.

## take

```
/uxmlevels take Steve mining 200
You took 200 from Steve on Mining.
```

Experience stops at zero; a level lost is lost. Rewards already taken stay taken.

## set

```
/uxmlevels set Steve mining 20
Steve is now level 20 in Mining.
```

The player lands at the start of the level, with no experience into it. A level outside 1 to the track's maximum
answers `admin.level-out-of-range`. `set` runs no automatic reward and moves no board: a manual reward at or below
the new level can then be claimed in the window.

## reset

Starts the player at level 1 with no prestige on every track, makes every reward claimable again, and removes them
from every board in every period. It does not stop a boost.

## boost

```
/uxmlevels boost Steve 2 1h
Steve now earns 2.00 times, for 1h.
```

The multiplier is above zero; the duration is a span such as `30m`, `2h` or `1d`. A new boost replaces the running
one. The boost is stored, so it survives a restart, and it counts down in real time whether the player is online or
not. An unreadable span answers `error.bad-duration`.

## doctor

```
How this plugin is
ok  storage: sqlite file levels.db, prefix uxmlevels_, schema v2
ok  windows: 4 read
ok  placeholders: published
Everything answered.
```

## When the database refuses

A `give`, `take`, `set` or `reset` the database refuses answers `error.storage-failed`, `The database refused that.
Nothing changed.`, and the console has the reason.

Related: [Overview](overview.md), [Multipliers and boosts](../tracks/multipliers-and-boosts.md), [Permissions](../permissions/reference.md)
