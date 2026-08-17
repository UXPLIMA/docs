---
title: Limits and costs
order: 603
description: The numeric nodes, their defaults, and how STACK, MAX and MIN combine them.
icon: sliders-horizontal
---

Limits, costs and delays are **entitlements**: a number resolved from a player's permissions. They are
declared in [`entitlements.yml`](../config/entitlements-yml.md), where each has a default value, a
combining strategy and a multiplier.

## The nodes

| Node | Controls | Default | Strategy | Rate |
|---|---|---|---|---|
| `uxmclaims.limit.claim.<n>` | Claims a player may own | `1` | STACK | ×1 |
| `uxmclaims.limit.chunk.<n>` | Chunks across all their claims | `10` | STACK | ×1 |
| `uxmclaims.limit.member.<n>` | Members in one claim | `50` | STACK | ×1 |
| `uxmclaims.limit.time.<n>` | Claim lifetime in seconds | `2592000` | STACK | ×1 |
| `uxmclaims.limit.warp.<n>` | Warps per claim | `3` | MAX | ×1 |
| `uxmclaims.limit.role.<n>` | Custom roles per claim | `5` | MAX | ×1 |
| `uxmclaims.limit.ban.<n>` | Bans per claim | `25` | MAX | ×1 |
| `uxmclaims.limit.invite.<n>` | Pending invitations per claim | `10` | MAX | ×1 |
| `uxmclaims.cost.claim.<level>.<price>` | Creating a claim | `0.0` | MIN | flat |
| `uxmclaims.cost.chunk.<count>.<price>` | Buying a chunk | `25.0` | MIN | flat |
| `uxmclaims.cost.warp.<count>.<price>` | Creating a warp | `0.0` | MIN | flat |
| `uxmclaims.cost.role.<count>.<price>` | Creating a role | `0.0` | MIN | flat |
| `uxmclaims.cost.warptp.<public\|private>.<price>` | Teleporting to a warp | `0.0` | MIN | flat |
| `uxmclaims.cost.invite.<count>.<price>` | Sending an invitation | `0.0` | MIN | flat |
| `uxmclaims.cost.rename.<price>` | Renaming a claim | `0.0` | MIN | flat |
| `uxmclaims.cost.time.<price>` | Extending, per second | `5.0` | MIN | flat |
| `uxmclaims.delay.teleport.<n>` | Teleport warmup in seconds | `3` | MIN | ×1 |

`2592000` seconds is 30 days.

## The three strategies

| Strategy | Combines several grants by | Right for |
|---|---|---|
| `STACK` | Adding them up | Limits that should accumulate across ranks |
| `MAX` | Taking the largest | Limits where the best rank should simply win |
| `MIN` | Taking the smallest | Costs and delays (smaller is the upgrade) |

A player holding both `uxmclaims.limit.claim.2` and `uxmclaims.limit.claim.3`:

- under `STACK`: `1 + 2 + 3 = 6` claims; the default is included
- under `MAX`: `3` claims
- under `MIN`: `2` claims

The default value participates in `STACK`, which is why `limit.claim` ships with a default of `1` and
not `0`.

## incrementRate

For **limits**, `incrementRate` multiplies the number in the node. With a rate of `20`,
`uxmclaims.limit.claim.5` grants 100 claims: the node becomes a *tier* rather than a count. All
shipped limits use rate `1`, so what you write is what you get.

For **costs**, `incrementRate` is a percentage increase per item already owned. With a rate of `10.0`
and a base of 100, the first claim costs 100, the second 110, the third 121. All shipped costs use
`0.0`, meaning flat pricing.

<Callout type="warning" title="MIN on costs means an ungranted rank is not the cheapest one">

Because costs use `MIN`, a player with no cost node pays `defaultValue`. Granting
`uxmclaims.cost.chunk.1.10` makes chunks cost 10 for that rank. Granting a rank a *higher* number than
the default does nothing at all: `MIN` keeps the default. Cost nodes can only ever discount.

</Callout>

## Worked example

```yaml
default:
  - uxmclaims.limit.claim.1     # 1 default + 1 = 2 claims
  - uxmclaims.limit.chunk.15    # 10 default + 15 = 25 chunks
vip:
  - uxmclaims.limit.claim.2     # …+2 = 4 claims when stacked on default
  - uxmclaims.limit.chunk.25    # …+25 = 50 chunks
  - uxmclaims.limit.warp.10     # MAX: 10 warps, not 13
  - uxmclaims.cost.chunk.1.10   # MIN: chunks cost 10 instead of 25
  - uxmclaims.delay.teleport.1  # MIN: 1 second warmup instead of 3
```

Because `limit.claim` and `limit.chunk` stack, a VIP who also holds the default group's nodes gets the
sum. If you inherit groups in LuckPerms, count the inherited nodes too: this is the single most
common source of "why does my VIP have twelve claims".
