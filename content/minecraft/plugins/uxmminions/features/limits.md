---
title: Limits
order: 307
description: The total per scope, quotas per type, per chunk, worlds, the bypass and granted slots.
icon: gauge
---

```hocon
limits {
  per-scope = 25
  per-type {
    spawner = 3
    seller  = 5
    feeder  = 5
  }
  per-chunk         = 8
  bypass-permission = "uxmminions.limits.bypass"
  whitelist         = false
  worlds            = ["world_the_end"]
}
```

| Key | Meaning |
|---|---|
| `per-scope` | The total one scope may place; a scope is an island where a claim plugin has one, a player otherwise |
| `per-type` | A quota per type beside the total; a type not named takes the total |
| `per-chunk` | How many may stand in one chunk |
| `bypass-permission` | Passes every cap |
| `whitelist`, `worlds` | `true` makes `worlds` the only worlds allowed; `false` makes them forbidden |

Checks run in order: world, total, type, chunk. `/uxmminions slots <player> <amount>` grants extra slots on top of the
total, for that player only. A quota you remove from `per-type` is not written back by an update.

`%uxmminions_placed%`, `%uxmminions_limit%`, `%uxmminions_remaining%` and their `_<type>` forms show where a player
stands.

Related: [Claims](claims.md), [Placeholders](../placeholders/reference.md)
