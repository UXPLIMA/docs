---
title: Block & Entity Limits
order: 720
description: Cap how many of a given block or entity each island may have.
icon: gauge
---

- **Block limits** are counted as players place and break the block, stored on the island, and synced across the network.
- **Entity limits** are enforced live from the entities currently on the island.
- A value of `-1` (or an absent key) means **unlimited**.

When a player tries to exceed a block limit, the placement is cancelled and they are told which block hit its cap. Entity spawns over the cap are silently cancelled.

## Configuration

```yaml
limits:
  # Set to an upgrades.yml key to raise caps as that upgrade levels up. Empty = off.
  upgrade-key: ""
  upgrade-bonus-per-level: 0
  blocks:
    HOPPER: 64
    SPAWNER: 8
    BEACON: 4
  entities:
    VILLAGER: 20
    IRON_GOLEM: 8
    SNOW_GOLEM: 16
```

### Scaling with upgrades

Set `upgrade-key` to an upgrade defined in `upgrades.yml` and every cap grows with that upgrade's level:

```
cap = base + round(upgrade-bonus-per-level * (level - 1))
```

For example, with `upgrade-key: size`, `upgrade-bonus-per-level: 8` and a base `HOPPER: 64`, an island at size level 3 may place `64 + 8 * 2 = 80` hoppers.

<Callout type="note" title="Use real block / entity names">

Keys must be valid 1.21.4 `Material` names (for blocks) and `EntityType` names (for entities).

</Callout>
