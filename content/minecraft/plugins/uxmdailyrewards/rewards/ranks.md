---
title: Ranks
order: 403
description: Where a player's rank comes from, the ladder, and how a day gives a rank something else.
icon: medal
---

```hocon
ranks {
  source            = "auto"
  permission-prefix = "uxmdailyrewards.rank."
  ladder            = ["default", "vip", "mvp"]
}
```

| Key | Default | What it does |
|---|---|---|
| `source` | `"auto"` | `auto`: LuckPerms when installed, permission nodes when not. `luckperms`: LuckPerms groups only. `permission`: nodes only. Read at start |
| `permission-prefix` | `"uxmdailyrewards.rank."` | On the permission path, this plus a rank id is the node of that rank: `uxmdailyrewards.rank.vip` |
| `ladder` | `["default", "vip", "mvp"]` | The ranks that count, worst first and best last. Empty gives everybody the same reward |

## Which rank a player claims at

The player's ranks are read when they claim, and the **last** rank of the ladder they hold wins. A group that is not
on the ladder is ignored. Rank ids are matched without regard to case.

| Source | A player holds a rank when |
|---|---|
| LuckPerms | The group is one of their own parent groups, or their primary group. A group reached only through another group's parents does not count |
| Permission | They have `<permission-prefix><rank>` |

`source = "luckperms"` without LuckPerms installed warns once at start, and no rank applies.

## What a rank gets

A day's `ranks.<rank>.reward` **replaces** the day's `reward` for that rank; it does not add to it, so repeat the
lines you still want. A rank the day does not name gets the day's `reward`. So `default` on the ladder needs no block
of its own, and a day that names only `mvp` gives `vip` players the default.

The calendar window shows each player the reward at their own rank.

## Operators

On the permission path an operator holds every node that is not declared otherwise, so an operator claims at the best
rank of the ladder. `uxmdailyrewards.rank.*` is declared, `op` by default, for a permission plugin that reads
wildcards.

Related: [Day files](day-files.md), [Permissions](../permissions/reference.md)
