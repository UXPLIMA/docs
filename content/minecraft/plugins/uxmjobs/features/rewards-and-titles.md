---
title: Rewards and titles
order: 409
description: What a level gives, automatic rewards, permissions granted by level, and the title ladder.
icon: award
---

## Level rewards

```hocon
rewards {
  "25" {
    icon      = "diamond"
    automatic = true
    actions   = [
      "[message] @reward.miner.25"
      "[permission] uxmjobs.perk.miner.deep"
      "[sound] entity.player.levelup"
    ]
  }
}
```

| Key | Meaning |
|---|---|
| `icon` | The material the [levels window](../menus/levels.md) draws |
| `automatic` | `true`: the actions run when the level is reached |
| `actions` | uxmlib-condition actions: `[message]`, `[broadcast]`, `[title]`, `[sound]`, `[console]`, `[give-money] <backend> <amount>`, `[permission] <node>` and the rest |
| `requirements` | Condition lines; checked for errors at load, not asked when the reward runs |

Every level a payment carries a player past runs its automatic reward, in order.

<Callout type="warning" title="automatic = false rewards are not given">

This build has no way to claim a reward written `automatic = false`: the levels window marks it "Waiting for you to
claim it", but no click or command runs its actions. Write `automatic = true` on every reward you want given. The two
highest rewards of every shipped job are written `false`: 50 and 100 for the miner, the top two for the others.

</Callout>

## Permissions by level

`[permission] some.node` grants a node while the player holds the level. Nothing is stored: the set is worked out from
the player's levels whenever it could change, so a level taken away takes its nodes with it. The grant lasts the
session; a permission plugin that wants it permanently reads the level placeholder instead.

## Titles

`titles.conf`:

```hocon
titles {
  default {
    "1"   = "@content.titles.titles.default.1"
    "25"  = "@content.titles.titles.default.25"
    "50"  = "@content.titles.titles.default.50"
    "75"  = "@content.titles.titles.default.75"
    "100" = "@content.titles.titles.default.100"
  }
  miner {
    "100" = "@content.titles.titles.miner.100"
  }
}
```

Read downwards: a player at level 43 wears the highest entry at or below 43. `default` covers every job; a job's own
block is asked first. A title is published as `%uxmjobs_title_<job>%`; a level with no title answers nothing.

Related: [Jobs and prices](jobs-and-prices.md), [titles.conf](../config/titles-conf.md)
