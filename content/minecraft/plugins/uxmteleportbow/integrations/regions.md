---
title: WorldGuard and Towny
order: 802
description: How a WorldGuard region or a Towny town closes a place to the bow.
icon: map
---

The [region filter](../bow/filters.md#regions) reads the names of the regions at a place from WorldGuard and from
Towny, whichever is installed, or both.

| Plugin | A place's names |
|---|---|
| WorldGuard | The id of every region that covers the block, as `/rg list` shows it |
| Towny | The name of the town whose claim covers the block; none in the wilderness |

Names are matched without regard to case. With both plugins, a place has the names from both, and it counts as
listed when any of them is in `filters.regions.list`.

## Closing the spawn

```
/rg define spawn
```

```hocon
filters {
  regions {
    enabled = true
    mode    = "deny"
    check   = "both"
    list    = ["spawn"]
  }
}
```

Run `/uxmteleportbow reload`. A player in the spawn cannot leave it with the bow (`refused-departure`), and an arrow
that lands in it does not bring anybody in (`refused-destination`).

## Without either plugin

With `enabled = true` and neither WorldGuard nor Towny on the server, the console says once at start that no region
is checked, and the filter lets everything through. The world filter still works.

Related: [Filters](../bow/filters.md), [Overview](overview.md)
