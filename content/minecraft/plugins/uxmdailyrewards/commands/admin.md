---
title: Operator commands
order: 303
description: Every /uxmdailyrewards subcommand, with its arguments, what it does and the permission it needs.
icon: shield
---

Every operator command is under `/uxmdailyrewards`, aliases `/uxmdr` and `/dailyrewardsadmin`.

| Command | Permission | What it does |
|---|---|---|
| `/uxmdailyrewards grant <player> <day>` | `uxmdailyrewards.admin.grant` | Runs that day's reward for an online player, at their rank, without moving their progress |
| `/uxmdailyrewards reset <player>` | `uxmdailyrewards.admin.reset` | Deletes the player's progress: their next claim is day 1 |
| `/uxmdailyrewards reload` | `uxmdailyrewards.admin.reload` | Reads `config.conf`, the days, the window, the messages and the theme again. See [Configuration](../config/index.md#reloading) |
| `/uxmdailyrewards doctor` | `uxmdailyrewards.admin.doctor` | Checks the storage, the window and PlaceholderAPI, one line each |
| `/uxmdailyrewards version` | `uxmdailyrewards.admin.version` | Prints the version this server runs |
| `/uxmdailyrewards` | `uxmdailyrewards.admin.version` | Points to `/uxmdailyrewards help` |

The language of another player is set under `/dailyrewards`: see [Player commands](player.md).

## grant

```
/uxmdailyrewards grant Steve 7
Day 7 is given to Steve.
```

`grant` runs the day's reward lines, as the player's rank reads them, with `%streak%` at the player's current streak.
It does not run the celebration, does not count as a claim and does not change the streak or the next day. The
player must be online. A day outside the calendar answers `admin.grant-no-such-day`: `Day 40 is outside a calendar of
28 days.`

Use it to make up a day: a player who left between the claim and the reward keeps the claim, and the console names
the day that was not given.

## reset

```
/uxmdailyrewards reset Steve
The progress of Steve is cleared.
```

`reset` takes the name of an online player or one the server remembers, and deletes their row: the streak and the
last claim. Their next claim is day 1, at once. A player with no row answers `admin.reset-nothing`; a name the server
has never seen answers `admin.player-offline`.

## doctor

```
How this plugin is
ok  storage: sqlite file dailyrewards.db, prefix uxmdailyrewards_, schema v1
ok  windows: 1 read
ok  placeholders: published
Everything answered.
```

The checks run off the main thread, since one of them opens a connection; the header shows at once and the lines
follow.

Related: [Overview](overview.md), [Permissions](../permissions/reference.md)
