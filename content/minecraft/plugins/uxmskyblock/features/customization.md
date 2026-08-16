---
title: Biome & Starter Chest
order: 710
description: 'Two ways to shape a fresh island: pick its biome and stock its starter
  chest.'
---

## Island biome

Players can change the biome of their island region.

| Command | Aliases | Description |
| --- | --- | --- |
| `/is biome <name>` | `biyom`, `biom` | Set your island's biome |
| `/is biome` | | List the allowed biomes |

- Requires the **TOGGLE_SETTINGS** permission.
- The biome is written into the world **once**, in small batches per tick, so it never freezes the server.
- On a proxy network the change runs on the backend that hosts the island; run it there (the plugin tells you which server if you're on the wrong one).
- The chosen name is stored so it can be shown anywhere via `%skyblock_biome%`.

<Callout type="tip" title="Re-enter to see it">

After the change finishes, walk out of and back into the area (or relog) so the client re-renders the new biome colors.

</Callout>

### Configuration

```yaml
biome:
  # Which biomes players may pick (must be valid 1.21.4 biome names).
  allowed:
    - PLAINS
    - FOREST
    - DESERT
    - JUNGLE
    - TAIGA
    - SNOWY_TAIGA
    - SAVANNA
    - SWAMP
    - BADLANDS
    - CHERRY_GROVE
    - MUSHROOM_FIELDS
    - WARM_OCEAN
  # How many 4-block columns to convert each tick. Higher = faster but heavier.
  columns-per-tick: 256
  # Vertical band around the island height that gets the new biome.
  band-below: 40
  band-above: 72
```

## Starter chest

Island schematics store the chest **block** but not its items, so a new island's chest arrives empty. The starter-chest feature fills the first chest found near the island center when the island is created.

### Configuration

```yaml
creation:
  starter-chest:
    enabled: true
    # Only fill if the chest is empty (never overwrite a stocked chest).
    only-if-empty: true
    # How far from the island center to look for the chest.
    search-radius: 16
    items:
      - "OAK_LOG 32"
      - "ICE 2"
      - "LAVA_BUCKET 1"
      - "MELON_SLICE 1"
      - "BONE_MEAL 16"
      - "OAK_SAPLING 4"
      - "SUGAR_CANE 3"
      - "RED_MUSHROOM 1"
      - "BROWN_MUSHROOM 1"
```

- Format is `MATERIAL AMOUNT` (or `MATERIAL:AMOUNT`); amount defaults to `1`.
- Invalid material names are skipped.
- Only newly created islands are affected: existing chests are never touched retroactively.
