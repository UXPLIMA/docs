---
title: Site preparation
order: 204
description: Clearing the footprint and laying a foundation before the first block.
icon: shovel
---

Before a build places its first block, the crew prepares the site: it removes what stands where the
schematic has air, then fills the gap under the floor down to the ground. A house built on a slope or in
a forest comes out as it looked in the schematic, not with trees through the living room or floating over
a hollow.

The boss bar reads **Preparing the site** while this runs.

## Clearing

Every cell the schematic leaves empty is cleared, from the top layer down so nothing is left hanging.

| Block found there | What happens |
|---|---|
| Air | Nothing |
| Water or lava | Left alone; draining it mid-build only floods the site |
| Unbreakable (bedrock, barriers, portal frames) | Left alone |
| A container | Removed, its contents dropped on the ground |
| Anything else | Removed without drops |

Clearing costs no material and no money. It counts toward the job's speed and the server's shared
budget like any other block.

## Foundation

For every column of the schematic's bottom layer that holds a block, the crew fills downward from just
under the floor until it reaches ground, up to `maxDepth` blocks. Air, liquids, grass, flowers and snow
count as gaps; anything solid ends the column.

Columns where the bottom layer is empty (a porch roof, an overhang) get no foundation.

The foundation goes below the box the protection check covered, so each column is asked about on its
own before anything is placed under it. A column the owner may not build in is skipped. With the
[warehouse](warehouse.md) on, each foundation block is charged like any other block.

## Settings

| Key | Default | Meaning |
|---|---|---|
| `sitePreparation.clear` | `true` | Clear the schematic's air cells |
| `sitePreparation.foundation.enabled` | `true` | Fill gaps under the floor |
| `sitePreparation.foundation.block` | `minecraft:dirt` | The block used, with optional states |
| `sitePreparation.foundation.maxDepth` | `6` | The deepest gap filled under one column |

## Notes

- Preparation only runs before the first building block. A job paused during it picks up where it was;
  a job that has started building never prepares again.
- A schematic saved with `/builders admin save` keeps the air in its selection, which is what tells the
  crew what to clear. A schematic whose air was stripped on export clears nothing.
- A foundation block that does not parse is logged once and foundations are skipped.

Related: [Building and performance](building.md), [Warehouse](warehouse.md)
