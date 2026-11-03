---
title: Filters
order: 303
description: The worlds the bow works in, and the WorldGuard regions or Towny towns a player may not leave or land in.
icon: filter
---

## Worlds

```hocon
filters {
  worlds {
    mode = "deny"
    list = []
  }
}
```

| Key | Default | What it does |
|---|---|---|
| `mode` | `"deny"` | `allow`: only a listed world works. `deny`: every world works except a listed one |
| `list` | `[]` | World names, matched without regard to case |

The shipped filter closes no world. A shot in a closed world is refused as the bow is released, with `refused-world`,
and `<world>` in its message is the world's name. With `item.give.respect-world-filter` on, a closed world is also one
where the bow is not handed out.

```hocon
# the bow in the lobby and nowhere else
worlds { mode = "allow", list = ["lobby"] }
```

## Regions

```hocon
filters {
  regions {
    enabled = false
    mode    = "deny"
    check   = "both"
    list    = []
  }
}
```

| Key | Default | What it does |
|---|---|---|
| `enabled` | `false` | Check regions at all |
| `mode` | `"deny"` | `allow` or `deny`, as for worlds |
| `check` | `"both"` | `departure`: the place the player leaves. `destination`: the place the arrow landed. `both`: both, at the cost of one more lookup per teleport |
| `list` | `[]` | Region names, matched without regard to case |

The regions are WorldGuard's, and Towny's towns: a block in a town's claim counts as a region named after the town.
With both plugins, a place is in the regions of both. A place counts as listed when any region there is listed.

| `mode` | A place with a listed region | A place with no listed region, or no region at all |
|---|---|---|
| `deny` | Refused | Allowed |
| `allow` | Allowed | Refused |

So `allow` with a list keeps the bow inside those regions, and refuses every shot that would leave or land outside.

```hocon
# nobody may teleport into or out of the shop and the spawn
regions { enabled = true, mode = "deny", check = "both", list = ["spawn", "shop"] }

# the bow works only inside the parkour arena
regions { enabled = true, mode = "allow", check = "destination", list = ["parkour"] }
```

## Notes

- **A typo in `mode` or `check`** is not an error: a `mode` the plugin cannot read is the shipped one, `deny`, and a
  `check` it cannot read is `both`.
- **Regions without WorldGuard or Towny.** With `enabled = true` and neither installed, the console says so once at
  start and no region is checked.

Related: [Shooting](shooting.md), [WorldGuard and Towny](../integrations/regions.md)
