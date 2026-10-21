---
title: block-values.yml
order: 770
description: What each block is worth in points, and how that drives island level.
icon: file-cog
---

`block-values.yml` sets how many **points** each block is worth. Points drive
island levels and the leaderboard, see [Points & Levels](../progression/points-levels.md).

---

## Format

```yaml
# Default points for blocks that aren't listed (0 = no points).
default-value: 0.0

values:
  NETHERITE_BLOCK: 10.0
  DIAMOND_BLOCK: 2.5
  QUARTZ_BLOCK: 1.5
  EMERALD_BLOCK: 1.5
  GOLD_BLOCK: 0.8
  IRON_BLOCK: 0.6
  COPPER_BLOCK: 0.6
  OBSIDIAN: 0.5
  AMETHYST_BLOCK: 0.4
  REDSTONE_BLOCK: 0.3
  LAPIS_BLOCK: 0.2
  COAL_BLOCK: 0.2
  OAK_LOG: 0.05
  GRASS_BLOCK: 0.02
  STONE: 0.01
```

| Key | Meaning |
|-----|---------|
| `default-value` | Points for any block not listed in `values` |
| `values.<MATERIAL>` | Points for that specific block (decimals allowed) |

Use Bukkit **material names** (uppercase, underscores).

---

## How Points Move

- **Place** a block → add its value to the island.
- **Break** a block → subtract its value.

So values are net: stacking diamond blocks raises your level; mining them lowers it
again.

---

## Tuning Tips

- Keep cheap, spammable blocks (stone, dirt) near **0** so players can't farm
  levels by placing junk.
- Reserve the big numbers for blocks that cost real resources (netherite, diamond).
- Set rare decorative items high if you want building to be rewarded.

<Callout type="tip">

`/is block` shows players these values in game. Reload with `/is admin reload`
after editing.

</Callout>
