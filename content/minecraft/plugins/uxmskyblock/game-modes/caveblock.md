---
title: CaveBlock
order: 50
description: An island enclosed in rock, entered in a carved room with tunnels, ravines, ores,
  structures, zones and pools to mine out to.
icon: mountain
---

A CaveBlock island is enclosed in rock. Players arrive in a room carved out of it, with tunnels
winding away and ravines cut deep through it, and mine outward through rock and ores. The rock is
closed in by a shell below, above and on every side.

## Turning it on

1. Set `enabled = true` in `modules/caveblock.conf`.
2. Add `caveblock` to `presets.modes` in `modules/presets.conf`.

The shipped preset `caveblock` starts with `"uxm:platform"` and `"uxm:cavern"`: the starter
platform, then the rock around it with the room carved out. In the Nether and the End the island
is enclosed in that dimension's own rock.

## The rock

| Setting | Shipped | What it does |
| --- | --- | --- |
| `shell` | `BEDROCK` | The block the rock is closed in by |
| `size.radius` | `32` | How far the rock reaches sideways from the arrival. Keep it inside half the grid spacing |
| `size.below`, `size.above` | `24`, `16` | How far it reaches down and up |
| `room.radius` | `4` | The room players arrive in |
| `tunnels.count`, `tunnels.length` | `6`, `40` | Tunnels that wind out of the room |
| `ravines.count`, `.length`, `.depth` | `2`, `30`, `14` | Ravines cut deep through the rock |

## Palettes

`palettes` holds the rock of each dimension by its id. A dimension not named uses the overworld's.

```hocon
overworld {
    rock = "STONE"
    ores = [ "COAL_ORE:0.014", "IRON_ORE:0.009", "GOLD_ORE:0.002:6" ]
    deep-rock = "DEEPSLATE"
    deep-below = 12
    deep-ores = [ "DEEPSLATE_DIAMOND_ORE:0.0015:16" ]
    richer = 1
}
```

- An ore is written `BLOCK:chance`, the chance one block of the rock is that ore, or
  `BLOCK:chance:below` for an ore found only that many blocks under the arrival and down.
- The chances of one rock add up to at most 1.
- `deep-rock` and `deep-ores` take over `deep-below` blocks under the arrival.
- `richer` makes ores richer further down: their chances rise evenly to `1 + richer` times as
  written at the floor of the rock. `0` keeps them as written.

## Structures

`structures` lays kinds of structure after the rock: `count` of each, spread evenly at `distance`
from the centre, with its floor `depth` blocks under the arrival. The shipped kinds are a
dungeon, two mineshafts, a geode and a trial chamber.

- A template is one of the server's own, a data pack's, or `schematic:<name>` for
  `schematics/<name>.schem`. The plugin writes the four shipped ones on its first start, to edit
  with any tool that writes a Sponge schematic or with `/is schematic save`.
- Inside a schematic, cave air is the hollow it carves out of the rock, and air leaves the rock as
  it stands.
- Each chest and barrel gets a loot table from `loot`, in turn, and each spawner the creature
  `spawner` names.
- A kind that would not fit inside the shell is not laid. Give a shipped kind `count = 0` to stop
  it.

## Zones

`zones` gives parts of the cave their own look. Each zone takes an even share of the round of the
island outside the room, turned by the island.

| Key | What it does |
| --- | --- |
| `biome` | The zone's biome. Empty leaves the island's own |
| `deep` | `true` lays the whole zone as the dimension's deep rock with its deep ores |
| `floor`, `on-floor` | A floor block laid by chance, and a block stood on it |
| `ceiling`, `under-ceiling` | A ceiling block, and a block hanging below it |
| `dimensions` | The dimensions the zone is in. `[]` stops a shipped zone |

The shipped zones are lush caves, dripstone caves and the deep dark.

## Pools

`pools` lays pools of liquid, each a flattened sphere `radius` blocks wide and half as high, its
middle between `depth-from` and `depth-to` blocks under the arrival, away from the room and the
shell. `fill` is how much of its height is the liquid, from the bottom: a lake is half full, a lava
pocket full and found by mining into it. Give a shipped pool `count = 0` to stop it.
