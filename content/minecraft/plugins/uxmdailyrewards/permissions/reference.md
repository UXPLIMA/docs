---
title: Reference
order: 701
description: Every node uxmDailyRewards declares, its default, what it grants, and the rank nodes.
icon: list
---

A node held by everybody (`default: true`) keeps its command visible, and a sender without it is refused with
`command.no-permission`; any other node hides its command from a sender without it.

## Players

| Node | Default | Grants |
|---|---|---|
| `uxmdailyrewards.use` | everyone | `/dailyrewards`: the calendar, the claim, and everything under it |
| `uxmdailyrewards.language` | everyone | `/dailyrewards lang`, `lang set`, `lang reset` |
| `uxmdailyrewards.rank.<rank>` | op, as any node not declared | That rank, on the permission path. The prefix is `ranks.permission-prefix` |
| `uxmdailyrewards.rank.*` | op | Every rank, on a permission plugin that reads wildcards |

## Operators

| Node | Default | Grants |
|---|---|---|
| `uxmdailyrewards.admin.grant` | op | `grant` |
| `uxmdailyrewards.admin.reset` | op | `reset` |
| `uxmdailyrewards.admin.reload` | op | `reload` |
| `uxmdailyrewards.admin.doctor` | op | `doctor` |
| `uxmdailyrewards.admin.version` | op | `version`, and the bare `/uxmdailyrewards` |
| `uxmdailyrewards.admin.language` | op | `/dailyrewards lang set-player` and `lang reset-player`, with `uxmdailyrewards.use` |
| `uxmdailyrewards.admin.*` | op | Every `admin` node, on a permission plugin that reads wildcards |

The two `*` nodes declare no children, so they grant the nodes under them only through a permission plugin that reads
a `*` as a wildcard, as LuckPerms does.

## Rank nodes

With `ranks.source` on `permission`, or on `auto` without LuckPerms, a player holds a rank when they have its node:

```
lp group vip permission set uxmdailyrewards.rank.vip true
lp group mvp permission set uxmdailyrewards.rank.mvp true
```

The best rank of the ladder a player holds wins, so an `mvp` who also has the `vip` node claims as `mvp`. With
LuckPerms installed and `source = "auto"`, the groups themselves are the ranks and these nodes are not read.

Related: [Ranks](../rewards/ranks.md), [Player commands](../commands/player.md)
