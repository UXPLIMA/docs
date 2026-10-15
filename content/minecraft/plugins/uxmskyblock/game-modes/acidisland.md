---
title: AcidIsland
order: 40
description: An island in an acid sea that burns whoever swims in it, with acid rain, a tide,
  vents, geysers and buried ruins.
icon: droplets
---

An AcidIsland island stands in an acid sea. The sea burns whoever swims in it, and acid rain burns
whoever it falls on. Water taken from the sea is acid too: a bottle of it poisons the one who
drinks it until it is made clean. Rain a cauldron catches is clean already, but a cauldron a
bucket of sea water was poured into holds the sea until it is emptied.

## Turning it on

1. Set `enabled = true` in `modules/acidisland.conf`.
2. Add `acid_island` to `presets.modes` in `modules/presets.conf`.

The shipped preset `acid_island` starts with `"uxm:platform"` and `"uxm:acid-sea"`, the action that
lays the sea around the island.

## The sea

The sea fills the air around the island and never replaces a block the island already has.

| Setting | Shipped | What it does |
| --- | --- | --- |
| `sea.below-island` | `2` | How many blocks below the island's start height the surface stands |
| `sea.depth` | `4` | How many blocks of water stand above the floor |
| `sea.radius` | `64` | How far from the centre the sea reaches. Keep it inside half the grid spacing |
| `sea.floor` | `SANDSTONE` | The block the sea stands on. A falling block is replaced by sandstone |
| `sea.bed` | | What the floor is made of, by weight, how thick it is and how high its mounds rise |

### Vents and geysers

A sulfur vent is magma on the floor that pulls a swimmer down, with fumes that reach whoever is
near. A geyser is soul sand that throws a swimmer up out of the sea. `vents.chance` and
`geysers.chance` are the chance a column of the floor is one, and `0` lays none. `vents.effects`
are the fumes' effects, each written `name:amplifier:seconds`.

### Offerings

An item a player throws into the sea that sinks onto a vent is an offering. If an offering in
`vents.offerings.list` names it, the vent takes as many as `takes` says, and a roll of that
offering's loot table comes back to whoever threw it while they are near. Only a player's own
throw counts. An item no offering takes dissolves as any other.

```hocon
offerings {
    after = "2s"
    list = [
        { items = ["minecraft:gold_ingot"], takes = 1, loot = "minecraft:chests/buried_treasure" }
    ]
}
```

### Buried ruins

`sea.buried` lays structures in the floor around the island, the way Poseidon lays its wrecks.
Each group lays `count` of its templates, `distance` blocks from the centre and spread evenly
around it, and gives each chest a loot table from `loot`. `groups = {}` buries nothing.

### The tide

The sea rises `tide.rise` blocks over half of `tide.period` and falls back over the other half, a
block at a time, by the clock, so every server of a network keeps the same tide. Water is added
only over the sea, never on an island. `rise = 0` keeps the sea still. The effect lists
`acid-tide-rising` and `acid-tide-falling` in `modules/effects.conf` fire at each block.

### One sea between islands

With `sea.between.enabled`, every chunk the server makes in the world `between.world` names is
laid with the sea, so the islands of that world stand in one sea that runs on as far as players
go, and a boat goes from island to island. Give the AcidIsland preset that world, keep other
modes out of it, and set `surface` to the islands' sea level. Chunks made before it was on are
left as they are, so start it on a new world.

### How it looks

`sea.look.biome` is painted over the sea when an island is made. The plugin's own
`uxmskyblock:sulfur_sea` is drawn in the colours below it, each written `#rrggbb`: `water`,
`water-fog`, `fog` and `sky`. The plugin writes it into a data pack at start, so a change to the
colours shows after a restart. Any biome the game or a data pack has may be named instead, and
`""` leaves the biome the world made.

## The hazard

| Setting | Shipped | What it does |
| --- | --- | --- |
| `hazard.check-every` | `1s` | How often each player on an AcidIsland island is checked |
| `hazard.water-damage` | `2.0` | Health taken at each check from a player in the sea. A heart is 2 |
| `hazard.rain-damage` | `1.0` | Health taken at each check from a player the rain falls on |
| `hazard.helmet-blocks-rain` | `true` | Whether anything worn on the head keeps the rain off |
| `hazard.water-protection` | `["water_breathing"]` | Effects that keep a player safe in the sea |
| `hazard.water-effects` | `["poison:0:3"]` | Effects the sea gives |
| `hazard.armour` | `netherite` | A full set of these keeps the acid off, losing `wear` durability per check |

## Sea life

At each check the sea looks within `sea-life.reach` blocks of each player. An item lying in the
sea dissolves after `items.after`. A creature in it takes `creatures.damage` at each check, except
those in `creatures.spared`, which live in it.

## Clean water

A bottle of acid water is made clean by smelting it in a furnace or brewing it with coal, as
`purification` allows. A bucket smelted comes out clean too. A bottle of clean water drunk heals
`clean-water.heals` health and gives `clean-water.effects`.
