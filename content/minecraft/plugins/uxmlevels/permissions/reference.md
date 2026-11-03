---
title: Reference
order: 601
description: Every node uxmLevels declares, its default, what it grants, and the nodes of tracks and multipliers.
icon: list
---

A node on a command hides it: a sender without it does not see the command.

## Players

| Node | Default | Grants |
|---|---|---|
| `uxmlevels.use` | everyone | `/levels`, `/levels level`, `/levels top`, and through the window every inner window |
| `uxmlevels.language` | everyone | `/levels lang`, `lang set`, `lang reset` |
| `uxmlevels.others` | op | `/levels level of` |
| `uxmlevels.prestige` | op | `/levels prestige`. The prestige window checks no node |

## Operators

| Node | Default | Grants |
|---|---|---|
| `uxmlevels.admin.give` | op | `give` |
| `uxmlevels.admin.take` | op | `take` |
| `uxmlevels.admin.set` | op | `set` |
| `uxmlevels.admin.reset` | op | `reset` |
| `uxmlevels.admin.boost` | op | `boost` and `unboost` |
| `uxmlevels.admin.reload` | op | `reload` |
| `uxmlevels.admin.doctor` | op | `doctor` |
| `uxmlevels.admin.version` | op | `version`, and the bare `/uxmlevels` |
| `uxmlevels.admin.language` | op | `/levels lang set-player` and `lang reset-player` |
| `uxmlevels.admin.*` | op | Every `admin` node, on a permission plugin that reads wildcards |

<Callout type="warning" title="The prestige window checks no node">

`uxmlevels.prestige` is `op` by default, but the prestige window behind the star in `/levels` lets any player
prestige. To keep prestige to the node, remove the `prestige` tile from `menus/main.conf`.

</Callout>

## Tracks

| Node | Default | Grants |
|---|---|---|
| `uxmlevels.track.<id>` | earned | Earning on that track. Set it to `false` to keep a player off it |
| `uxmlevels.track.*` | everyone | Earning on every track. Set it to `false` to keep a player off all of them |

A track's own node is asked first, then the wildcard; a player with neither set earns. Both are read when the player
joins.

## Multipliers

The nodes of `experience.permission-multipliers` are yours to name: `uxmlevels.multiplier.vip` and
`uxmlevels.multiplier.mvp` as shipped. They are not declared, so most permission plugins give them to nobody but
operators until you grant them. The best one held wins.

```
lp group vip permission set uxmlevels.multiplier.vip true
lp group builder permission set uxmlevels.track.combat false
```

Related: [Multipliers and boosts](../tracks/multipliers-and-boosts.md), [Player commands](../commands/player.md)
