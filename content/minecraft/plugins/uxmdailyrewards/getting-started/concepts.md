---
title: Concepts
order: 203
description: The calendar, the claim window and the grace, the streak, the four states of a day, and ranks.
icon: lightbulb
---

## The calendar

The calendar is `calendar.length` days long, 28 by default, and each day is a file in `days/`. A player's first
claim is day 1, the next day 2, and so on; after the last day the calendar wraps to day 1 while the streak keeps
counting. Day 29 of a streak is day 1 of the calendar again. See [The calendar](../rewards/calendar.md).

## The window and the grace

Two spans decide when a player may claim and when they lose the run:

| Setting | Default | Means |
|---|---|---|
| `claim.window` | `24h` | How long one claim holds a player off. They may claim again once it has passed |
| `claim.grace` | `24h` | How much longer the streak survives after the window. Later than window plus grace, the streak is lost |

With the defaults a player claims once a day and keeps the streak if they come back within 48 hours of their last
claim. The spans count from the moment of the last claim, not from midnight: a player who claims at 21:00 may claim
again from 21:00 the next day.

## The streak

The streak is how many days in a row a player has claimed. A claim within the grace adds one; a claim after it starts
again at day 1 with a streak of 1, and the player reads `claim.streak-reset` first. An operator can put a player back
to nothing with `/uxmdailyrewards reset`.

## The four states of a day

| State | Means | Shipped icon |
|---|---|---|
| Ready | The day the player may claim now | Chest minecart, glowing |
| Next up | The next day, while the window still runs | Command block minecart |
| Taken | A day already claimed in this round of the calendar | Minecart |
| Not yet | A later day | Furnace minecart |

See [Calendar window](../menus/calendar.md).

## Ranks

A day can give a rank something else. The ranks are a ladder in `config.conf`, worst first; a player claims at the
best rank on the ladder they hold, read from LuckPerms or from permission nodes. A rank a day does not name gets the
day's default. See [Ranks](../rewards/ranks.md).

## Lines

A reward is a list of action lines, the same lines every UXM plugin runs: `[console] give %player% diamond 2`,
`[give-money] exp 110`, `[subtitle] ...`, `[sound] ...`. `%player%`, `%day%` and `%streak%` are filled in before a
line runs. See [Day files](../rewards/day-files.md).
