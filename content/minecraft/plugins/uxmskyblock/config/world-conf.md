---
title: world.conf
order: 657
description: "The island world: spawn height, the spawn and starter structures."
icon: globe
---

The island world: where a new island's spawn sits, the spawn in its middle, and the structures a void world never needs.

The file is `plugins/uxmSkyblock/modules/world.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `island-spawn-y` | `100` | How high above the void a new island's spawn sits. This is the height the starter schematic is pasted at, so move it with the schematic. |
| `suppressed-structures` | a list of 6 | The middle of an island world is its spawn, where a player with no island stands, and no island is ever placed there. Where nothing stands under the spawn the plugin lays the ground schematics/world_spawn.schem describes, a plain platform it writes on its first start. Replace the file with a lobby of your own, or build one at the spawn: ground that is already there is left alone. |
