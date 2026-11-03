---
title: Player commands
order: 202
description: Every /jobs and /contracts subcommand, with its arguments and what it does.
icon: user
---

No node is needed for anything on this page.

## /jobs

| Command | What it does |
|---|---|
| `/jobs` | Opens the [job board](../menus/jobs.md) |
| `/jobs mine` | The jobs you work, with level and progress, as chat lines |
| `/jobs browse` | Every job the server has, with its description |
| `/jobs menu <job>` | Opens [one job's window](../menus/job.md) |
| `/jobs join <job>` | Takes a job; see [Joining and leaving](../features/joining-and-leaving.md) |
| `/jobs leave <job>` | Leaves a job and pays the leave cost; a dialog asks first when dialogs are on |
| `/jobs quit <job>` | Says what leaving would cost and what the archive keeps, without leaving |
| `/jobs stats <job>` | Level, experience and what is left to the next level |
| `/jobs points` | Your points balance |
| `/jobs top [job] [period]` | The leaderboard in chat: a job id or `total` (the default), and `alltime` (the default), `monthly`, `weekly` or `daily` |
| `/jobs board` | Opens the [leaderboard window](../menus/top.md) |
| `/jobs why` | Why the last piece of work paid what it paid |
| `/jobs boosters` | What is boosting you, as lines |
| `/jobs boosts` | The same, as a [window](../menus/boosters.md) |
| `/jobs toggle` | The payment line on the action bar, on or off |
| `/jobs bar` | The progress boss bar, on or off |
| `/jobs archive` | What is kept of the jobs you left |
| `/jobs shop` | Opens the [job shop](../menus/shop.md) |
| `/jobs buy <entry>` | Buys one shop line by its id, without the window |

### why

```
Why mine:diamond_ore
placed block ledger : paid
protection : ...
...
It paid 23.5 money.
```

A line per step of the [payment order](../features/payments.md), then the result. Before any work it says "Nothing has
paid you yet."

### top

`/jobs top miner weekly` prints this week's best miners, `leaderboard.chat-lines` (10) of them. An all time board ranks
by level; a month, week or day ranks by experience earned in it. Right after a restart it may say the board is being
worked out.

### toggle and bar

The action bar line starts on (`display.action-bar.default-on`) and the boss bar off (`display.boss-bar.default-on`).
The choice is kept while the server runs and is forgotten on a restart.

## /contracts

| Command | What it does |
|---|---|
| `/contracts` | The contracts you may sign and the ones you have, as lines |
| `/contracts menu` | Opens the [contracts window](../menus/contracts.md) |
| `/contracts mine` | What you signed: points earned of the goal, hours left |
| `/contracts sign <contract>` | Signs one and pays its cost |
| `/contracts cancel <contract>` | Walks away from one; nothing is refunded |
| `/contracts shift` | Whether the working hours of a signed contract are open right now |

## Language

`/uxmjobs lang` says what language you read; `/uxmjobs lang set <language>` and `/uxmjobs lang reset` choose and clear
it. They need no node. See [Languages](../config/languages.md).

Related: [Admin commands](admin.md), [Contracts](../features/contracts.md)
