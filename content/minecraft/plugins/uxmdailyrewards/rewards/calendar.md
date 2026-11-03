---
title: The calendar
order: 401
description: The length, the claim window and the grace, how the streak counts, and what happens at the end of the calendar.
icon: calendar
---

```hocon
claim {
  window = "24h"
  grace  = "24h"
}

calendar {
  length = 28
}
```

| Key | Default | What it does |
|---|---|---|
| `claim.window` | `"24h"` | How long one claim holds a player off; more than zero |
| `claim.grace` | `"24h"` | How much longer the streak survives after the window; `0s` makes the window itself the deadline |
| `calendar.length` | `28` | Days in one round; each needs `days/<number>.conf`. At least `1` |

A span is written as a number and a unit: `90m`, `24h`, `1d`, `500ms`. A span the plugin cannot read is reported in
the console and the default is used.

## How a claim is decided

Counting from the moment of the player's last claim:

| Time since the last claim | The claim |
|---|---|
| No claim yet | Day 1, streak 1 |
| Less than `window` | Refused with `claim.too-soon` and the time left |
| From `window` to `window + grace` | The next day, streak plus one |
| `window + grace` or more | Day 1, streak 1, after `claim.streak-reset` |

The day is the streak counted around the calendar: with 28 days, streak 1 is day 1, streak 28 is day 28, and streak
29 is day 1 again. The streak itself keeps counting, so `%streak%` can say a player has come 60 days in a row while
they claim day 4.

## Changing the length

A longer calendar needs a file for every new day; a shorter one simply stops using the days past its end. A player's
day is worked out from their streak each time, so after a change a player in the middle of a round continues on the
day their streak lands on in the new length.

## A day without a file

A day with no file, or a file that cannot be read, is named in the console at start and on reload. Days 1 to 28 are
shipped, and a shipped day file you delete is written back at the next start; so a day goes missing when the calendar
is longer than 28 days, when a file is deleted and reloaded, or when it cannot be read. A player whose
next day it is cannot claim: they read `claim.no-such-day`, their streak and their day stay as they were, and the
window keeps running towards the grace. Fix the file and reload.

## Time zones

Everything counts from the moment of the last claim, in milliseconds, never from a date. A server that changes time
zone, or a network whose servers sit in different ones, hands out no free day.

Related: [Day files](day-files.md), [Calendar window](../menus/calendar.md)
