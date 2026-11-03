---
title: Multipliers and boosts
order: 305
description: The four multipliers an earned amount passes through, permission multipliers, and timed boosts.
icon: x
---

Every earned amount is multiplied by four numbers, then rounded down:

| Multiplier | From | Default |
|---|---|---|
| Global | `experience.global-multiplier` | `1.0` |
| Permission | The best of `experience.permission-multipliers` the player holds | `1.0` |
| Boost | A timed multiplier from `/uxmlevels boost` | `1.0` |
| Prestige | `1 + bonus-per-prestige x prestige count` on that track | `1.0` |

Their product is the player's multiplier, shown as `%uxmlevels_<track>_multiplier%` and in the levels window.
Experience given with `/uxmlevels give` is not multiplied.

## Permission multipliers

```hocon
experience {
  permission-multipliers {
    "uxmlevels.multiplier.vip" = 1.25
    "uxmlevels.multiplier.mvp" = 1.5
  }
}
```

A node and its multiplier, as many as you like. The best one a player holds wins; three nodes do not stack. The nodes
are yours to name and to grant. They are read when the player joins: a node granted to a player who is online counts
from their next join.

## Boosts

```
/uxmlevels boost Steve 2 1h
/uxmlevels unboost Steve
```

A boost is one multiplier for one player for a span: `30m`, `2h`, `1d`. A second boost replaces the first. It is
stored in the database, counts down in real time whether the player is online or not, and on a network reaches every
server sharing the database. `uxmlevels.admin.boost` grants both commands.

A boost for a whole event, such as a double experience weekend, is a command per player, or the global multiplier
changed and reloaded.

Related: [Prestige](prestige.md), [config.conf](../config/config-conf.md#experience)
