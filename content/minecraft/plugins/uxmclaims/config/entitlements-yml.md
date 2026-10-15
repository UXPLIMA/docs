---
title: entitlements.yml
order: 503
description: "Limits, costs, delays and cooldowns: their defaults, strategies and rates."
icon: sliders-horizontal
---

Every number a player's rank can change lives here. This page covers the file's shape and how a value
is resolved; [Limits and costs](../permissions/limits.md) covers granting the nodes.

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
| `strategy` | How several matching nodes combine: `STACK`, `MAX` or `MIN`. Limits and delays only. |
| `defaultValue` | What a player with no matching node gets |
| `incrementRate` | A multiplier for limits and delays, a percentage for costs |

The map key is the permission **prefix**. A missing or incomplete entry falls back to the shipped
default for that prefix, and the console warns once:
`entitlements.yml has no complete '<prefix>' entry; using the shipped default.`

## Limits and delays

A player holds `<prefix>.<amount>`, for example `uxmclaims.limit.claim.5`. Each amount is multiplied
by `incrementRate`, then the matches combine:

| Strategy | Result | Use for |
|---|---|---|
| `STACK` | `defaultValue` plus every match | Limits that accumulate across ranks |
| `MAX` | The largest match, never below `defaultValue` | Limits where the best rank wins |
| `MIN` | The smallest match, never above `defaultValue` | Delays and cooldowns, where smaller is the perk |

`STACK` includes the default: `uxmclaims.limit.claim` ships with `1`, so a rank granting `.2` gives
three claims. A rate above 1 turns the node into a tier: with rate `20`, `uxmclaims.limit.claim.5`
gives 100 claims. Every shipped limit and delay uses rate `1`.

## Costs

Costs ignore `strategy`. A node carries the price, and the lowest matching price wins:

| Node shape | Used by | Example |
|---|---|---|
| `<prefix>.<n>.<price>` | `claim`, `chunk`, `warp`, `role`, `invite` | `uxmclaims.cost.chunk.1.10`: chunks cost 10 from the 1st on |
| `<prefix>.*.<price>` | the same | `uxmclaims.cost.warp.*.0`: every warp is free |
| `<prefix>.<public\|private>.<price>` | `warptp` | `uxmclaims.cost.warptp.public.5` |
| `<prefix>.<price>` | `rename`, `time` | `uxmclaims.cost.time.2_5`: 2.5 per second |

`<n>` is which item is being bought: the 3rd claim, the 12th chunk. Write a decimal point as `_`. For
item `n`, a node naming `n` exactly sets the price; otherwise the nearest lower node (or
`defaultValue` from item 1) is the base, and `incrementRate` compounds per item after it:

| Rate | Base | 1st | 2nd | 3rd |
|---|---|---|---|---|
| `0.0` | 100 | 100 | 100 | 100 |
| `10.0` | 100 | 100 | 110 | 121 |

A cost node can raise a price as well as lower it: it replaces `defaultValue` rather than competing
with it.

## The shipped defaults

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
| `uxmclaims.delay.rename` | MIN | `30` | `1` |
| `uxmclaims.delay.warprename` | MIN | `30` | `1` |
| `uxmclaims.delay.rolerename` | MIN | `30` | `1` |
| `uxmclaims.delay.relocate` | MIN | `60` | `1` |
| `uxmclaims.delay.warprelocate` | MIN | `60` | `1` |
| `uxmclaims.delay.transfer` | MIN | `300` | `1` |
| `uxmclaims.delay.invite` | MIN | `10` | `1` |
| `uxmclaims.delay.flags` | MIN | `5` | `1` |

`delay.teleport` is the warmup before a teleport; the other delays are cooldowns between two uses of
the same action, all in seconds. A cooldown refusal answers with `errorOnCooldown`.

## Notes

- **Whose nodes count differs.** Limits are read from the claim owner, costs and delays from the
  player acting, and a resolved limit of `0` means unlimited for some limits and none for others.
  [Limits and costs](../permissions/limits.md) has the details.
- **Operators, `uxmclaims.admin` and `<prefix>.*` skip the numbers.** They get unlimited limits, free
  costs and no delays.
- **Without LuckPerms every player gets `defaultValue`.** The nodes are read through LuckPerms; see
  `permissionSupport` in [config.yml](config-yml.md).
- **`defaultValue: 1` for claims is deliberately tight.** Raise it if your server expects players to
  hold several claims without buying a rank.
- **`limit.time` interacts with `expireMode`.** With `claimSettings.expireMode: false` nothing expires
  and the entitlement is inert. With it on, `limit.time` caps how far `/claim extend` may push the
  expiry: going past it answers `errorClaimTimeExceeded`.
- **Watch inherited groups.** With `STACK`, a VIP inheriting the default group gets the sum of both
  sets of nodes. This is the usual explanation for a limit that came out higher than intended.
