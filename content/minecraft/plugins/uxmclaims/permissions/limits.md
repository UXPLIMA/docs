---
title: Limits and costs
order: 603
description: The numeric nodes, their defaults, and how a player's grants combine into one number.
icon: sliders-horizontal
---

Limits, costs, delays and cooldowns are **entitlements**: a number resolved from a player's
permissions. Each one is declared in [`entitlements.yml`](../config/entitlements-yml.md) with a default
value, a combining strategy and an `incrementRate`. They are read from LuckPerms; without it, or with
`permissionSupport: false`, every player gets the default (see
[What reads the nodes](reference.md#what-reads-the-nodes)).

## Limits

| Node | Controls | Read from | Default | Strategy |
|---|---|---|---|---|
| `uxmclaims.limit.claim.<n>` | Claims a player may own | The player creating, or receiving a transfer | `1` | STACK |
| `uxmclaims.limit.chunk.<n>` | Chunks in one claim | Claim owner | `10` | STACK |
| `uxmclaims.limit.member.<n>` | Members in one claim | Claim owner | `50` | STACK |
| `uxmclaims.limit.time.<n>` | Most remaining time a claim may hold after `/claim extend`, in seconds | Claim owner | `2592000` | STACK |
| `uxmclaims.limit.warp.<n>` | Warps in one claim | Claim owner | `3` | MAX |
| `uxmclaims.limit.role.<n>` | Custom roles in one claim | Claim owner | `5` | MAX |
| `uxmclaims.limit.ban.<n>` | Bans in one claim | Claim owner | `25` | MAX |
| `uxmclaims.limit.invite.<n>` | Pending invitations in one claim | Claim owner | `10` | MAX |

`2592000` seconds is 30 days. A resolved limit of `0` means no limit for `claim`, `chunk` and `time`,
and means none allowed for the others.

## Costs

| Node | Charges for | Default |
|---|---|---|
| `uxmclaims.cost.claim.<n>.<price>` | Creating your `n`th claim | `0.0` |
| `uxmclaims.cost.chunk.<n>.<price>` | Growing a claim to `n` chunks | `25.0` |
| `uxmclaims.cost.warp.<n>.<price>` | Creating a claim's `n`th warp | `0.0` |
| `uxmclaims.cost.role.<n>.<price>` | Creating a claim's `n`th custom role | `0.0` |
| `uxmclaims.cost.invite.<n>.<price>` | Inviting when the claim would reach `n` members | `0.0` |
| `uxmclaims.cost.warptp.<public\|private>.<price>` | Teleporting to a public or a private warp | `0.0` |
| `uxmclaims.cost.rename.<price>` | Renaming a claim | `0.0` |
| `uxmclaims.cost.time.<price>` | Extending a claim, per second per chunk | `5.0` |

Costs are always read from the player who acts. Write a decimal price with an underscore,
`uxmclaims.cost.chunk.1.12_5` for 12.5, because a dot would be read as a separator. A `*` in place of
`n` matches every count.

## Delays and cooldowns

| Node | Controls | Default |
|---|---|---|
| `uxmclaims.delay.teleport.<n>` | Teleport warmup, in seconds | `3` |
| `uxmclaims.delay.rename.<n>` | Cooldown between claim renames | `30` |
| `uxmclaims.delay.warprename.<n>` | Cooldown between warp renames | `30` |
| `uxmclaims.delay.rolerename.<n>` | Cooldown between role renames | `30` |
| `uxmclaims.delay.relocate.<n>` | Cooldown between `/claim setspawn` uses | `60` |
| `uxmclaims.delay.warprelocate.<n>` | Cooldown between warp moves | `60` |
| `uxmclaims.delay.transfer.<n>` | Cooldown between ownership transfers | `300` |
| `uxmclaims.delay.invite.<n>` | Cooldown between invitations sent | `10` |
| `uxmclaims.delay.flags.<n>` | Cooldown between flag changes | `5` |

Every delay uses `MIN`, counts seconds, and applies to the player who acts.

## How limits and delays combine

| Strategy | Result | Shipped on |
|---|---|---|
| `STACK` | The default plus every granted number | `claim`, `chunk`, `member`, `time` |
| `MAX` | The largest granted number, never below the default | `warp`, `role`, `ban`, `invite` |
| `MIN` | The smallest granted number, never above the default | Every delay |

A player holding `uxmclaims.limit.claim.2` and `uxmclaims.limit.claim.3`, with a default of `1`:

| Strategy | Claims |
|---|---|
| `STACK` | `1 + 2 + 3 = 6` |
| `MAX` | `3` |
| `MIN` | `1`, because the default is the smallest |

`incrementRate` multiplies the number in a limit or delay node. With a rate of `20`,
`uxmclaims.limit.claim.5` grants 100 claims and the node becomes a tier. Every shipped entry uses `1`.

## How costs resolve

Costs ignore `strategy`. For the `n`th item, uxmClaims uses the cheapest node granted for exactly `n`.
With none, it takes the nearest granted count below `n` and, when `incrementRate` is above `0`, raises
that price by `incrementRate` percent for every step from there. With no node at all, it starts from
`defaultValue` at count 1. With a rate of `10.0` and a default of 100, claims cost 100, 110, 121.

A granted price replaces the default, so a node can make an action dearer as well as cheaper.

## Unlimited and free

| Holding | Limits | Costs | Delays and cooldowns |
|---|---|---|---|
| Operator or `uxmclaims.admin` | Unlimited | `0` | `0` |
| `<prefix>.*`, such as `uxmclaims.limit.warp.*` | That one is unlimited | That one is free | That one is `0` |

## Worked example

```yaml
default:
  - uxmclaims.limit.claim.1     # 1 default + 1 = 2 claims
  - uxmclaims.limit.chunk.15    # 10 default + 15 = 25 chunks per claim
vip:
  - uxmclaims.limit.claim.2     # +2 = 4 claims when stacked on default
  - uxmclaims.limit.chunk.25    # +25 = 50 chunks per claim
  - uxmclaims.limit.warp.10     # MAX: 10 warps, not 13
  - uxmclaims.cost.chunk.1.10   # every chunk costs 10 instead of 25
  - uxmclaims.delay.teleport.1  # MIN: 1 second warmup instead of 3
```

<Callout type="warning" title="Inherited groups stack">

A VIP who inherits the default group holds both sets of nodes, and `STACK` adds them all. If you
inherit groups in LuckPerms, count the inherited nodes too: this is the usual reason a VIP ends up
with more claims than you planned.

</Callout>
