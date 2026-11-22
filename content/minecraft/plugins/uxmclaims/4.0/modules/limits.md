---
title: Limits
order: 1313
description: How permission nodes set each player's caps and cooldowns, and the limits.yml file behind them.
icon: gauge
---

Other modules register their caps and cooldowns here: how many lands a player may own, how many chunks a land
may have, how long a rename cooldown lasts. The limits module reads `limits.yml` and the permission nodes each
player holds, and gives every module the player's value. `/uxm limits check <player>` shows the result.

Module `limits` · enabled by default · `modules/limits/limits.yml`

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm limits check <player>` | Lists every limit with the online player's value, the default and the strategy. | `uxmclaims.admin.limits` |

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin.limits` | op | `/uxm limits check`. |

A limit's name is also the prefix of the nodes that change it. For the limit `uxmclaims.limit.chunk`:

| Node a player holds | Gives |
|---|---|
| `uxmclaims.limit.chunk.25` | `25`, times the entry's `increment`. `_` is the decimal point: `uxmclaims.delay.teleport.1_5` gives `1.5`. |
| `uxmclaims.limit.chunk.vip` | The value `limits.yml` lists for that node under `values`. A named node not listed there does nothing. |
| `uxmclaims.limit.chunk.*` | No limit. For a `min` entry (a cooldown) it gives `0`, so no wait. |

Only nodes set to true count. A node with a negative value also means no limit.

## Strategies

Every entry in `limits.yml` has a strategy that says how the default and the player's nodes combine:

| Strategy | Result | Used for |
|---|---|---|
| `stack` | The default plus every node added together. `default: 10` with `.5` and `.20` gives 35. | Lands, chunks, members, land time. |
| `max` | The highest node, never below the default. | Warps, roles, bans, invites. |
| `min` | The lowest node, never above the default. | Cooldowns and warm-ups, where lower is better. |

A player with no matching node gets the default. A negative default means unlimited (`0` for `min`).

## Settings

`limits.yml` holds one list, `entitlements`. Each entry:

| Key | Default | What it does |
|---|---|---|
| `node` | (required) | The limit name and node prefix. Do not rename it. |
| `strategy` | (required) | `stack`, `max` or `min`. It replaces the strategy the owning module registered. |
| `default` | (required) | The value for a player with no node. Delays are in seconds. |
| `increment` | `1` | What a numbered node is multiplied by (3.x `incrementRate`, also read). With `increment: 5`, `uxmclaims.limit.chunk.2` gives 10. |
| `values` | none | Named nodes for ranks: `- { node: uxmclaims.limit.chunk.vip, value: 40 }`. |

```yaml
entitlements:
  - node: uxmclaims.limit.chunk
    strategy: stack
    default: 10
    increment: 1
    values:
      - { node: uxmclaims.limit.chunk.vip, value: 40 }
```

Write decimals in `default`, `increment` and `value` with a dot (`2.5`). In YAML an unquoted `2_5` is the number
25; `_` stands for the dot only inside node names.

## Registered limits

These are the limits the bundled modules register, with the file's shipped defaults. A limit only exists while
its module runs.

| Limit | Default | Strategy | What it caps | Module |
|---|---|---|---|---|
| `uxmclaims.limit.claim` | `1` | stack | Lands a player owns. | [Lands](lands.md) |
| `uxmclaims.limit.chunk` | `10` | stack | Chunks in one land. | [Lands](lands.md) |
| `uxmclaims.limit.member` | `50` | stack | Members of one land. | [Lands](lands.md) |
| `uxmclaims.limit.role` | `5` | max | Custom roles of one land. | [Lands](lands.md) |
| `uxmclaims.limit.time` | `2592000` | stack | Land time a land may hold ahead, in seconds (30 days). | [Expiry](expiry.md) |
| `uxmclaims.limit.warp` | `3` | max | Warps of one land. | [Warps](warps.md) |
| `uxmclaims.limit.ban` | `25` | max | Active bans of one land. | [Bans](bans.md) |
| `uxmclaims.limit.invite` | `10` | max | Open invites from one land. | [Invites](invites.md) |
| `uxmclaims.limit.area` | `max-per-land` | max | Areas in one land. | [Areas](areas.md) |
| `uxmclaims.limit.plot` | `5` | max | Plots a player owns or rents. | [Plots](plots.md) |
| `uxmclaims.limit.ally`, `.enemy` | `ally.max`, `enemy.max` | max | Allies and enemies of one holder. | [Relations](relations.md) |
| `uxmclaims.limit.town-chunk` | `16` | stack | Chunks of a town. | [Towns](towns.md) |
| `uxmclaims.limit.outpost` | `1` | max | Outposts of a town. | [Towns](towns.md) |
| `uxmclaims.delay.teleport` | `3` | min | Warm-up before `/claim spawn` and warp teleports. | [Lands](lands.md), [Warps](warps.md) |
| `uxmclaims.delay.rename` | `30` | min | Cooldown between land renames. | [Lands](lands.md) |
| `uxmclaims.delay.rolerename` | `30` | min | Cooldown between role renames. | [Lands](lands.md) |
| `uxmclaims.delay.relocate` | `60` | min | Cooldown between land spawn moves. | [Lands](lands.md) |
| `uxmclaims.delay.transfer` | `300` | min | Cooldown between land transfers. | [Lands](lands.md) |
| `uxmclaims.delay.flags` | `5` | min | Cooldown between flag changes. | [Lands](lands.md) |
| `uxmclaims.delay.unstuck` | `60` | min | Cooldown of `/claim unstuck`. | [Lands](lands.md) |
| `uxmclaims.delay.warprename`, `.warprelocate` | `30`, `60` | min | Cooldowns for renaming and moving a warp. | [Warps](warps.md) |
| `uxmclaims.delay.invite` | `10` | min | Cooldown between two invites. | [Invites](invites.md) |

A registered limit missing from the file is added at its registered default when the module loads, so the file
always lists every limit that can apply.

## Placeholders

With the [PlaceholderAPI](placeholders.md) module:

| Placeholder | Renders |
|---|---|
| `%uxmclaims_player_limit_<name>%` | The player's value of `uxmclaims.limit.<name>`, online players only. `limit_chunk` reads `uxmclaims.limit.chunk`. |

## Notes

- With the limits module off, every limit is its module's registered default and permission nodes are ignored.
- Without the [LuckPerms](luckperms.md) module, nodes are read only from online players. An offline player (a
  land owner who is away, for example) gets the default.
- Prices are not limits. `uxmclaims.cost.*` nodes are set up in `costs.yml` of the [Economy](economy.md) module.
- Other modules can add to a value after the nodes are read: [Claim Points](claim-points.md) add chunks per
  land, [Levels](levels.md) add town chunks and outposts.
- 3.x shipped `uxmclaims.limit.time` as `2592000000`. It is read as seconds now (about 82 years) and logged as
  `W-ENT-LIMIT-TIME-MS`; set `2592000` for 30 days.
- `/uxm reload` reads `limits.yml` again.

Related: [Economy](economy.md), [LuckPerms](luckperms.md), [Claim Points](claim-points.md), [Permissions](../permissions.md)
