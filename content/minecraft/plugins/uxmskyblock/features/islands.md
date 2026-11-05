---
title: Islands
order: 401
description: The grid, size and border, the spawn and homes, biomes, the Nether and the End.
icon: map
---

## The grid

Islands stand on a square spiral from 0,0, `grid.spacing` blocks apart (5120 as shipped). The spacing changes only
while no island stands away from the centre. When an island is deleted, reset or recycled, everything it wrote is
cleared, plus `grid.clear-margin` blocks past it (16) for water, lava and trees that reached over. See
[config.conf](../config/config-conf.md).

## Size and border

An island's size is its radius, set by the tier of its [Island Size upgrade](upgrades.md): 25 blocks at the first tier
and 100 at the fifth, as shipped. Nobody builds past it but staff with `uxmskyblock.admin.bypass`. The border is drawn
as a world border that each player shows or hides and colours for themselves in the
[border window](../menus/island.md#border). How the edge looks to everyone (a border, a wall of mist, a wall of glass,
or nothing) is chosen there too. What happens at the edge is in [settings.conf](../config/settings-conf.md).

## Spawn and homes

`/is setspawn` moves where every member and visitor arrives. Each player saves named homes with `/is sethome <name>`
and goes back with `/is gohome <name>`; how many is in `homes` in [config.conf](../config/config-conf.md).

## Biomes

The [biomes window](../menus/island.md#biomes) changes the whole island's biome: Plains, Desert, Jungle, Swamp, Snowy
Plains, Flower Forest and Nether Wastes as shipped, each with what an island must reach first. See
[biomes.conf](../config/biomes-conf.md).

## The Nether and the End

`/is nether` and `/is end` go to the island's other worlds. Whether each island has its own, everyone shares one, or
there is none is set in [dimensions.conf](../config/dimensions-conf.md).

## Flying

`/is fly` flies on an island you belong to, when your role holds Fly on the island and you hold
`uxmskyblock.island.fly`. Leaving the island sets you down, and the fall is spared. See [fly.conf](../config/fly-conf.md).
