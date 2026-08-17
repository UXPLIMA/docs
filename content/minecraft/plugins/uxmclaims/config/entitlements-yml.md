---
title: entitlements.yml
order: 503
description: Limits, costs and delays — their defaults, strategies and multipliers.
icon: sliders-horizontal
---

Every number a player's rank can change lives here. The node list with defaults is in
[Limits and costs](../permissions/limits.md); this page is about the file's own shape.

## An entry

```yaml
entitlements:
  uxmclaims.limit.claim:
    strategy: STACK
    defaultValue: 1
    incrementRate: 1
```

| Key | Meaning |
|---|---|
| `strategy` | How several matching permissions combine — `STACK`, `MAX` or `MIN` |
| `defaultValue` | What a player with no matching permission gets |
| `incrementRate` | A multiplier for limits, a percentage for costs |

The map key is the permission **prefix**. A player grants the value by holding
`<prefix>.<amount>` — `uxmclaims.limit.claim.5`.

## strategy

| Value | Combines by | Use for |
|---|---|---|
| `STACK` | Adding every match, plus the default | Limits that accumulate across ranks |
| `MAX` | Taking the largest match | Limits where the best rank wins outright |
| `MIN` | Taking the smallest match | Costs and delays, where smaller is the perk |

`STACK` includes `defaultValue` in the sum. That is why `uxmclaims.limit.claim` ships with a default
of `1` and a rank granting `.2` produces three claims, not two.

## incrementRate

Two different meanings depending on what the entitlement is.

**For limits, it multiplies.**

| Rate | Node | Result |
|---|---|---|
| `1` | `uxmclaims.limit.chunk.500` | 500 chunks |
| `20` | `uxmclaims.limit.claim.5` | 100 claims |

A rate above 1 turns the node into a *tier* — level 5 rather than five claims. Every shipped
entitlement uses `1`, so what you write is what you get.

**For costs, it is compound interest per item owned.**

| Rate | Base | 1st | 2nd | 3rd |
|---|---|---|---|---|
| `0.0` | 100 | 100 | 100 | 100 |
| `10.0` | 100 | 100 | 110 | 121 |

Every shipped cost uses `0.0`, meaning flat pricing. Setting a rate is how you make the tenth claim
cost meaningfully more than the first without writing ten permission nodes.

## The shipped defaults, in one place

| Prefix | Strategy | Default | Rate |
|---|---|---|---|
| `uxmclaims.limit.claim` | STACK | `1` | `1` |
| `uxmclaims.limit.chunk` | STACK | `10` | `1` |
| `uxmclaims.limit.member` | STACK | `50` | `1` |
| `uxmclaims.limit.time` | STACK | `2592000` | `1` |
| `uxmclaims.limit.warp` | MAX | `3` | `1` |
| `uxmclaims.limit.role` | MAX | `5` | `1` |
| `uxmclaims.limit.ban` | MAX | `25` | `1` |
| `uxmclaims.limit.invite` | MAX | `10` | `1` |
| `uxmclaims.cost.claim` | MIN | `0.0` | `0.0` |
| `uxmclaims.cost.chunk` | MIN | `25.0` | `0.0` |
| `uxmclaims.cost.warp` | MIN | `0.0` | `0.0` |
| `uxmclaims.cost.role` | MIN | `0.0` | `0.0` |
| `uxmclaims.cost.warptp` | MIN | `0.0` | `0.0` |
| `uxmclaims.cost.invite` | MIN | `0.0` | `0.0` |
| `uxmclaims.cost.rename` | MIN | `0.0` | `0.0` |
| `uxmclaims.cost.time` | MIN | `5.0` | `0.0` |
| `uxmclaims.delay.teleport` | MIN | `3` | `1` |

## Notes

- **`defaultValue: 1` for claims is deliberately tight.** Every player gets one claim and nothing
  else until a rank says otherwise. Raise it if your server expects players to hold several without
  buying a rank.

- **`limit.time` interacts with `expireMode`.** With `claimSettings.expireMode: false` nothing
  expires and the entitlement is inert. With it on, `limit.time` caps how far `/claim extend` may
  push the expiry — extending past it answers *"The maximum claim duration has been exceeded."*

- **Cost nodes can only ever discount,** because they use `MIN`. Granting a rank a number higher than
  `defaultValue` changes nothing.

- **Watch inherited groups.** With `STACK`, a VIP inheriting the default group gets the sum of both
  sets of nodes. This is almost always the explanation for a limit that came out higher than intended.
