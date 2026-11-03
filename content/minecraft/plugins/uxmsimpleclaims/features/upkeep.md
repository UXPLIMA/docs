---
title: Upkeep
order: 308
description: Taxes, time limited claims and inactivity, each off by default.
icon: hourglass
---

## Tax

```hocon
tax { enabled = false, per-chunk = 5.0, period-days = 7, grace-hours = 72 }
```

Every `period-days` the owner pays `per-chunk` for each chunk. An owner who has not paid when `grace-hours` run out
loses their newest chunk, then the next one a period later. The inbox warns them.

## Time limited claims

```hocon
expiry { time-limited = false, start-days = 14, daily-per-chunk = 2.0, warn-days = 3, max-days = 90 }
```

A claim lasts `start-days`, and `/claim extend <days>` buys more at `daily-per-chunk` for each chunk, up to `max-days`
ahead. The owner is warned `warn-days` before it ends.

## Inactivity

```hocon
inactivity { enabled = false, days = 60, action = "delete" }
```

The claims of an owner away for `days` are deleted, or with `action = "transfer"` given to their highest member.
`uxmsimpleclaims.inactivity.exempt` keeps a player's claims.

A claim lost to any of these is kept as a [snapshot](staff-tools.md#snapshots).

Related: [config.conf](../config/config-conf.md#tax-expiry-and-inactivity)
