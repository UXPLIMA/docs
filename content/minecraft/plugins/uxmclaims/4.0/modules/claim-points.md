---
title: Claim Points
order: 1315
description: Extra chunks players earn by playing, on top of their permission-based chunk limit.
icon: hourglass
---

Claim points reward play time with land. Every hour a player is active earns points, and each point is one more
chunk for the lands they own, the way GriefPrevention hands out claim blocks. `/claim points` shows a player
what they have; admins give or take points by command.

Module `claim-points` · disabled by default · `modules/claim-points/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim points` | Shows your points, your counted play time and the minutes to the next point. | `uxmclaims.command.claim-points.use` |
| `/uxm claim-points check <player>` | Shows another player's points and play time. | `uxmclaims.admin.claim-points` |
| `/uxm claim-points give <player> <amount>` | Gives points on top of what the player earned. | `uxmclaims.admin.claim-points` |
| `/uxm claim-points take <player> <amount>` | Takes points from the player. | `uxmclaims.admin.claim-points` |

The admin commands take any player who has played on the server, online or not. The local names of these
commands are in `modules/claim-points/lang/<language>/commands.yml`.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.claim-points.use` | everyone | `/claim points`. |
| `uxmclaims.admin.claim-points` | op | `/uxm claim-points check`, `give` and `take`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `points-per-hour` | `1.0` | Points earned per hour of play. `0.5` is one point every two hours. |
| `max-earned` | `100` | Most points play time can earn; `0` is no cap. Points given by command come on top of the cap. |
| `afk-minutes` | `5` | Minutes without moving after which play time stops counting; `0` counts idle time too. |
| `limits` | `[uxmclaims.limit.chunk]` | The limits the points add to. `uxmclaims.limit.chunk` is the chunk limit of one land. |
| `save-minutes` | `5` | Minutes between saves of play time to the database. Play time is also saved when the module stops. |

## How points add up

A player's points are their earned points (play time x `points-per-hour`, rounded down, capped at
`max-earned`) plus the points given by command, minus the points taken, never below zero. Play time counts in
whole minutes for every online player who moved within `afk-minutes`.

The points are added after the player's own limit is worked out from `limits.yml` and their permission nodes
(see [Limits](limits.md)). With the defaults, a player whose chunk limit is 10 and who has 4 points may claim
14 chunks in each personal land they own.

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_claim-points_points%` | The player's points (also `%uxmclaims_claim-points%`). |
| `%uxmclaims_claim-points_minutes%` | Counted play time in minutes. |
| `%uxmclaims_claim-points_hours%` | Counted play time in whole hours. |
| `%uxmclaims_claim-points_next%` | Minutes until the next earned point; empty at the cap. |

These need the [PlaceholderAPI](placeholders.md) module and the PlaceholderAPI plugin.

## Notes

- Points add to every personal land the player owns, each on its own, not to one shared budget. Town and admin
  lands get nothing.
- Taking more points than a player has leaves them at zero and keeps the rest as a debt: points they earn later
  first pay it off.
- Players cannot buy points yet.
- In network mode an account is saved whole: if two servers change one player's points at the same moment, the
  later save wins.
- The [Importers](importers.md) module turns GriefPrevention claim blocks into claim points.

Related: [Limits](limits.md), [Lands](lands.md), [Importers](importers.md)
