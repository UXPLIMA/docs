---
title: Expiry
order: 1303
description: Lands that run out of time unless their owners buy more.
icon: hourglass
---

With expiry on, every new land gets a time limit and is deleted when the time runs out. Owners and members with the right role buy more time with `/claim extend` or from the time menu that `/claim time` opens. Owners are reminded before their land expires.

Module `expiry` · enabled by default · `modules/expiry/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim time` | Shows how long the land has left and, if you may extend it, opens the time menu | none |
| `/claim extend <duration>` | Buys more time for the land, such as `1d12h30m` | `uxmclaims.ability.claim.reschedule` |
| `/uxm expiry info <land>` | Shows when a land expires | `uxmclaims.admin.expiry` |
| `/uxm expiry set <land> <time\|never>` | Sets the time a land has left, or makes it never expire | `uxmclaims.admin.expiry` |
| `/uxm expiry add <land> <duration>` | Adds time to a land without charging anyone | `uxmclaims.admin.expiry` |

`/claim time` and `/claim extend` act on the land you stand in, else your only land.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.ability.claim.reschedule` | everyone | Extend the time of lands you manage |
| `uxmclaims.admin.expiry` | op | The `/uxm expiry` commands and extending any land |

| Role permission | Allows |
|---|---|
| `expiry:manage` (3.x `MANAGE_TIME`) | Buy time for the land |

| Limit or cost | Default | Applies to |
|---|---|---|
| `uxmclaims.limit.time.<seconds>` | 2592000 (30 days), stack | Most time a land may have left after an extension, read from the owner |
| Cost `expiry:time` | 5 | Price per second of extra time, per chunk or per land (see `time-cost-mode`) |

| Flag | Default | What it does |
|---|---|---|
| `expiry:shield` | off, admin only | A land with the flag on never expires |

## Settings

| Key | Default | What it does |
|---|---|---|
| `initial-duration` | `30d` | Time a new land starts with; `never` turns expiry off for new lands |
| `time-cost-mode` | `per-chunk` | `per-chunk`: every chunk pays the price per second. `per-land`: one price whatever the size |
| `sweep-interval` | `5s` | How often lands are checked for running out of time |
| `exempt-admin-lands` | `true` | Server-owned lands never expire |
| `assign-missing-on-start` | `true` | At start, lands without an end date (made while the module was off) get `initial-duration` |
| `broadcast-expired` | `true` | Tell everyone online when a land expires |
| `reminders.enabled` | `true` | Remind online owners before their land expires |
| `reminders.on-login` | `true` | Remind owners on join when their land is within the largest reminder time |
| `reminders.times` | `[1d, 1h, 10m]` | Time left at which an owner is reminded, once each |
| `reminders.check-interval` | `1m` | How often online owners are checked |

`/uxm reload` applies changes; the two timer intervals need a restart.

## Placeholders

All four read the player's current land: the one picked with `/claim select`, the one they stand in, or their only land.

| Placeholder | Renders |
|---|---|
| `%uxmclaims_expiry_remaining%` | Time left, as text |
| `%uxmclaims_expiry_seconds%` | Seconds left, `-1` when the land never expires |
| `%uxmclaims_expiry_date%` | Date the land expires |
| `%uxmclaims_expiry_expired%` | `true` when the time has run out |

## Notes

- Expiry compares the stored date with the clock, so a stopped server does not shift an end date; a land past its date is deleted at the first check after start.
- An expired land is deleted like any other, so it can be brought back with `/claim admin restore` within the lands retention time.
- On a network only the leading server deletes expired lands; reminders go to players on every server.
- Setting `initial-duration: never` keeps existing end dates. Use `/uxm expiry set <land> never` to clear one.

Related: [Lands](lands.md), [Economy](economy.md), [Ruins](ruins.md)
