---
title: Points & Levels
order: 640
description: 'An island''s level is the heart of progression: it gates upgrades and
  decides its rank on the leaderboard. Levels come from points, and points come from
  the blocks placed on the island.'
---

---

## Points

Every block type has a point value in
[block-values.yml](../configuration/block-values-yml.md):

- **Place** a block → its points are **added** to the island.
- **Break** a block → the same points are **removed**.

Blocks not listed are worth `default-value` (0 by default; they give nothing).

```yaml
values:
  DIAMOND_BLOCK: 2.5
  IRON_BLOCK: 0.6
  STONE: 0.01
  OBSIDIAN: 0.5
```

So a diamond block is worth 250× a stone block. Players climb by building with
**valuable** blocks, not just lots of cheap ones.

---

## Levels

Total points are turned into a **level** using thresholds in
[levels.yml](../configuration/levels-yml.md):

```yaml
requirements:
  1: 100
  2: 250
  3: 500
  4: 1000
  5: 2000
  ...
```

When an island's points cross a threshold, it reaches that level.

### Automatic levels

Beyond the highest defined threshold, levels are generated automatically:

```yaml
auto:
  enabled: true
  step: 8000          # base points added per new level
  multiplier: 1.15    # step grows by this each level
```

This means the curve keeps climbing smoothly forever; you don't have to hand-write
hundreds of levels.

---

## Seeing Your Progress

| Command | Shows |
|---------|-------|
| `/is level` | Your current level and points |
| `/is top` | The server leaderboard |
| `/is block` | What each block is worth |

Placeholders `%skyblock_level%` and `%skyblock_points%` are available through
[PlaceholderAPI](../integrations/placeholderapi.md).

---

## How Tracking Works

Block placement and breaking are tracked live as players build, so points update
in real time. Generators and crops that produce blocks count too, which is why
the [generator upgrade](generators.md) is such a powerful way to level.
