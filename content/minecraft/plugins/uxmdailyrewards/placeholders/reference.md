---
title: Reference
order: 801
description: Every %uxmdailyrewards_...% placeholder, what it renders, and what it answers for a player who is away.
icon: braces
---

uxmDailyRewards registers the `uxmdailyrewards` expansion when PlaceholderAPI is installed. Every placeholder answers
from memory, from the row read when the player logged in and kept up to date by their claims, so a scoreboard that
asks many times a second costs nothing.

| Placeholder | Renders | Player away |
|---|---|---|
| `%uxmdailyrewards_streak%` | The current streak; `0` once it has run out or before the first claim | `0` |
| `%uxmdailyrewards_day%` | The day the player may claim next, or is waiting for | `1` |
| `%uxmdailyrewards_available%` | `true` when a day is ready now | `true` |
| `%uxmdailyrewards_time_left%` | The time until the next claim, such as `5h 12m 3s`; empty when a day is ready | empty |
| `%uxmdailyrewards_seconds_left%` | The same in whole seconds; `0` when ready | `0` |

A player who is away answers as a player who never claimed, because their row is not kept in memory. A key the
plugin does not have is left in the text as written.

## Examples

```
Streak: %uxmdailyrewards_streak% days
Next reward in %uxmdailyrewards_time_left%
```

A condition for a plugin that shows a line only when a reward is waiting: `%uxmdailyrewards_available% == true`.

## On a network

The row in memory is the one this server read at login and wrote at each claim. A claim on another server of the
network shows here when the player next logs in, opens the calendar or claims. The claim rule itself always reads the
database, so a player can never claim twice on two servers.

Related: [PlaceholderAPI](../integrations/placeholderapi.md)
