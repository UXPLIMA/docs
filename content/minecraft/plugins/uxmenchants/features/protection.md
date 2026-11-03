---
title: Protection
order: 311
description: Worlds where nothing happens, the nineteen claim plugins, the block break treaty, and blocks no effect touches.
icon: shield-check
---

## Worlds

```hocon
worlds {
  mode = "blacklist"
  list = []
}
```

`mode` is `blacklist` or `whitelist`; `list` names worlds. A world that is off loses both halves: no enchantment fires
there and no route hands one out there, the anvil and a clicked book included.

## Claims

Block effects (`vein-miner`, `tree-feller`, `smelt-drops`, `extra-drops`) read land claims through the shared claim
seam, which covers nineteen plugins: WorldGuard, GriefPrevention, Lands, Towny, Factions, Residence, PlotSquared,
HuskClaims, HuskTowns, Kingdoms, BentoBox, SuperiorSkyblock2, GriefDefender, SimpleClaimSystem, ExcellentClaims,
Homestead, RClaim, XClaim and uxmClaims. A block the player may not break is skipped.

| Key | Meaning |
|---|---|
| `claims.combine` | `any-land` or `all-land`, when two claim plugins cover one block |
| `claims.providers.<name>` | `false` ignores that plugin even when it is installed |

With none installed nothing happens: no error, no log line, no feature lost.

## The block break treaty

A vein miner and a tree feller break blocks in a loop, and two plugins doing that would break one block twice. A block
one of them broke carries the metadata key `blockbreakevent-ignore`, and a block that already carries it is skipped.
This plugin reads the key and writes it, marking each block before it breaks it and releasing it after.

```hocon
block-break-treaty {
  enabled      = true
  metadata-key = "blockbreakevent-ignore"
}
```

Turn it off only if nothing else on the server honours it.

## Blocks no effect touches

```hocon
effects {
  materials {
    whitelist = []
    blacklist = ["BEDROCK", "BARRIER", "END_PORTAL_FRAME", "SPAWNER", "REINFORCED_DEEPSLATE"]
  }
}
```

When `whitelist` holds anything, only those blocks are touched. An effect's own `materials` block may narrow this and
never widen it. `explode` breaks no block at all, because a vanilla explosion cannot be routed through these checks.

Related: [Effects](effects.md#materials), [Integrations](../integrations/overview.md)
