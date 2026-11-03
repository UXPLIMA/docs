---
title: Contest commands
order: 203
description: Every /contest subcommand, with its arguments, node and what it does.
icon: trophy
---

| Command | Node | What it does |
|---|---|---|
| `/contest` | `uxmplots.contest` | Every contest and its phase; the [contest window](../menus/contests.md) for a player |
| `/contest info <contest>` | `uxmplots.contest` | One contest: theme, phase, time left, fee, room |
| `/contest entries <contest>` | `uxmplots.contest` | What is entered |
| `/contest scores <contest>` | `uxmplots.contest` | What it has scored so far |
| `/contest enter <contest> [team]` | `uxmplots.contest.enter` | Enters the plot you stand on, or takes a fresh plot; pays the fee |
| `/contest invite <contest> <player>` | `uxmplots.contest.enter` | Asks somebody onto your team |
| `/contest join <contest>` | `uxmplots.contest.enter` | Accepts an invitation |
| `/contest leave <contest>` | `uxmplots.contest.enter` | Takes your build out |
| `/contest vote [contest] [stars]` | `uxmplots.contest.vote` | Alone, goes to the next entry and shows its [scorecard](../menus/contests.md#scorecard); with stars, scores the entry you stand in. No contest named takes the one being judged |
| `/contest tour [contest]` | `uxmplots.contest.vote` | Takes you to the next entry you have not judged |
| `/contest hall` | `uxmplots.contest.hall` | Opens the [hall of fame](../menus/contests.md#hall-of-fame) |
| `/contest edit <contest>` | `uxmplots.contest.edit` | Builds and changes a contest in game (operators) |

`info`, `entries`, `scores` and the bare command run from the console. See [Contests](../features/contests.md).

Related: [Contests](../features/contests.md)
