---
title: Player commands
order: 202
description: Every /dailyrewards subcommand, with its arguments, what it does and the permission it needs.
icon: user
---

`/dailyrewards`, aliases `/daily` and `/rewards`. Every line needs `uxmdailyrewards.use`, which everyone has.

| Command | Also needs | What it does |
|---|---|---|
| `/dailyrewards` | | Opens the [calendar](../menus/calendar.md). Player only |
| `/dailyrewards claim` | | Claims the ready day without opening anything. Player only |
| `/dailyrewards lang` | `uxmdailyrewards.language` | Says which language you read uxmDailyRewards in, and which the server has. Player only |
| `/dailyrewards lang set <language>` | `uxmdailyrewards.language` | Reads uxmDailyRewards in that language from now on. Player only |
| `/dailyrewards lang reset` | `uxmdailyrewards.language` | Forgets your choice; you read your client's language again. Player only |
| `/dailyrewards lang set-player <player> <language>` | `uxmdailyrewards.admin.language` | Sets another player's language |
| `/dailyrewards lang reset-player <player>` | `uxmdailyrewards.admin.language` | Clears another player's choice |

## claim

`claim` does what a click on the ready day does. The answers:

| Reply | Key | When |
|---|---|---|
| `You claimed day 1. Your streak is 1.` | `claim.claimed` | The claim went through; the reward and the celebration follow |
| `Your streak ran out, so you start again at day 1.` | `claim.streak-reset` | Before `claimed`, when the last claim was longer ago than the window plus the grace |
| `You have claimed today. Come back in 23h 59m 41s.` | `claim.too-soon` | The window has not passed |
| `Day 5 has no reward set up. Tell an operator.` | `claim.no-such-day` | The day has no file, or its file could not be read. Nothing is claimed |
| `Your claim could not be saved, so nothing was given. Try again.` | `claim.not-stored` | The database refused the write |
| `Your claim is already being handled. Wait a moment.` | `claim.in-progress` | A second claim arrived while the first was being written |

A claim is written to the database before anything is given, so a crash between the two never pays twice.

Related: [Overview](overview.md), [The calendar](../rewards/calendar.md), [Languages](../config/languages.md)
