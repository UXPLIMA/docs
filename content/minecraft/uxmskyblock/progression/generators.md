---
title: Generators
order: 660
description: The generator upgrade controls what a player's cobblestone generator
  produces. Each tier is a set of weighted block chances — higher tiers swap plain
  cobble for ores, and the top tier produces real obsidian.
---

Tiers are defined under `upgrades.generator` in
[upgrades.yml](../configuration/upgrades-yml.md).

---

## Default Tiers

<Tabs>
<Tab label="Tier 1 (Lvl 0)">

| Block | Chance |
|-------|--------|
| Cobblestone | 100% |

</Tab>
<Tab label="Tier 2 (Lvl 6, 8k)">

| Block | Chance |
|-------|--------|
| Cobblestone | 80% |
| Coal Ore | 15% |
| Iron Ore | 5% |

</Tab>
<Tab label="Tier 3 (Lvl 18, 25k)">

| Block | Chance |
|-------|--------|
| Cobblestone | 60% |
| Iron Ore | 20% |
| Gold Ore | 15% |
| Diamond Ore | 5% |

</Tab>
<Tab label="Tier 4 (Lvl 35, 70k)">

| Block | Chance |
|-------|--------|
| Cobblestone | 38% |
| Iron Ore | 25% |
| Gold Ore | 20% |
| Diamond Ore | 12% |
| Emerald Ore | 3% |
| Obsidian | 2% |

</Tab>
<Tab label="Tier 5 (Lvl 50, 150k)">

| Block | Chance |
|-------|--------|
| Cobblestone | 25% |
| Iron Ore | 20% |
| Gold Ore | 18% |
| Diamond Ore | 15% |
| Emerald Ore | 7% |
| Obsidian | 14% |
| Ancient Debris | 1% |

</Tab>
</Tabs>

The top tier is a genuine "lava obsidian" generator that regularly produces
obsidian (and a touch of ancient debris) instead of plain cobblestone.

---

## How Weighting Works

`chances` are **weights**, not strict percentages — when a generator block forms,
one entry is rolled in proportion to its weight. Weights don't have to sum to 100,
but using values that add to 100 makes them easy to read as percentages.

```yaml
generator:
  type: generator
  levels:
    2:
      required-level: 6
      required-money: 8000
      chances:
        COBBLESTONE: 80
        COAL_ORE: 15
        IRON_ORE: 5
```

---

## The Obsidian Bucket Trick

When lava meets water it can form **obsidian**, and the lava would normally be
lost. With `obsidian.bucket-to-lava: true` (default), right-clicking an obsidian
block with an **empty bucket** breaks it and hands the lava back as a lava bucket —
so an obsidian farm never wastes a drop.

```yaml
obsidian:
  bucket-to-lava: true
```

On an island the player needs the `BLOCK_BREAK` permission for this; off an island
it always works.

<Callout type="info" title="Vanilla reminder">

Flowing lava + water = cobblestone; a lava **source** + water = obsidian (the
source is consumed). The generator's obsidian chance plus this bucket trick let
high-level islands farm obsidian sustainably.

</Callout>
