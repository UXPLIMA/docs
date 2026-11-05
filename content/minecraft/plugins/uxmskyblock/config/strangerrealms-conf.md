---
title: strangerrealms.conf
order: 647
description: "StrangerRealms: real land with an Upside Down."
icon: flip-vertical
---

StrangerRealms: an island of real land, with an Upside Down in place of the Nether. The Upside Down is the island's land mirrored block for block and distressed: the grass dead, the trees bare, the flowers withered. It is laid when the island is made, in the world dimensions.conf names for the Nether, so the Nether of a StrangerRealms island is its Upside Down.

The file is `plugins/uxmSkyblock/modules/strangerrealms.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether islands can be made as StrangerRealms islands. |
| `upside-down.radius` | `48` | How far from the island's centre the land is mirrored, in blocks. Every chunk it reaches is copied whole, from the lowest height both worlds share to the highest. |
| `upside-down.palette` | a list of 13 | How the mirror distresses what it copies. Each rule is FROM:TO: a block, or a name with one * such as *_LEAVES, turned into another block. The first rule that matches wins, and a block no rule matches is mirrored as it is. |
| `upside-down.late-copy` | a list of 44 | What a mirror laid after the island was made copies. The island grows past the radius above with its members and its size, and its land there is mirrored the first time it loads. By then players may have built on it, and a block copied across would be had twice, once on each side. So such a mirror copies only the ground written here, a name or a name with one *, and lays air for the rest: what players put down, ore, and anything that holds things. Keep the list to plain ground. |

## `mobs`

What the Upside Down makes of a creature born in it, on a StrangerRealms island's land. The Upside Down is the Nether's world, so what would be born there is Nether-born: here it is turned into the creature of the overworld it has become.

| Key | Default | What it does |
|---|---|---|
| `reasons` | `NATURAL`, `CHUNK_GEN`, `SPAWNER` | The ways of being born that are turned: NATURAL, CHUNK_GEN, SPAWNER, and any other the server names. A creature a command or a plugin brings is left alone. |
| `turn` | a list of 9 | Each rule is FROM:TO: a creature, or a name with one * such as *_PIGLIN, turned into another. TO may be NONE, which keeps the creature from being born at all. The first rule that matches wins, and a creature no rule matches is born as it is. |
| `effects` | `speed:0:3600`, `resistance:0:3600` | What every creature born there carries, each written name:amplifier:seconds. |

## `glimmer`

The glimmer: a light set on a StrangerRealms island's land shines through at the same place in the Upside Down, and one set in the Upside Down shines through on the land, for as long as it stands. The glimmer is light without a block, so nothing can be taken or built on it.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether lights glimmer across at all. |
| `lights` | a list of 13 | The blocks that are lights: a name, or a name with one * such as *_CANDLE. |
| `level` | `12` | How bright the glimmer is, from 1 to 15. |

## `switches`

Switches linked across the veil. A switch below used on a StrangerRealms island acts at the same place on the other side, on the land or in the Upside Down, on whatever of its kind stands there: a lever or a button passes on its power, and a door, trapdoor or gate stands open or shut as the one used does. Each is named, or a pattern with *. blocks = [] links none.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `blocks` | `LEVER`, `*_BUTTON`, `*_DOOR`, `*_TRAPDOOR`, `*_FENCE_GATE` |  |

## `warped-compass`

The warped compass: held on a StrangerRealms island, it points at the nearest player on the other side of the veil, at the same place on this side. With nobody over there it spins. Its name and its lines come out of messages/, in the language of the player who makes it.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether the compass can be made and points at all. |
| `check-every` | `1s` | How often the compass turns to where the players are. |
| `ingredients` | `COMPASS`, `WARPED_FUNGUS` | What a crafting grid holds, in any order, to make one. One to nine items. |

## `claim`

How far a StrangerRealms island reaches. It starts at what its size upgrade gives it and reaches further with every member past the first, up to the limit. A size bought past the limit is kept.

| Key | Default | What it does |
|---|---|---|
| `per-member` | `8` | How many blocks each member past the first adds to the radius. |
| `max-radius` | `200` | The furthest an island reaches however many members it has. Keep it inside half the grid spacing, so two islands never meet. |

## `border`

The border of the StrangerRealms land. It holds the furthest StrangerRealms island with room around it and grows as islands are made, on this server or any other that shares the database. It is set only in the worlds below: the Nether is shared with every other mode's islands, so the Upside Down is never given one.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether the plugin sets the border at all. |
| `check-every` | `60s` | How often the furthest island is read and the border moved. |
| `center-x` | `0` | Where the border is centred, which is the centre of the island grid. |
| `center-z` | `0` |  |
| `margin` | `512` | The room kept beyond the furthest island's edge, in blocks. |
| `minimum` | `2048` | The narrowest and the widest the border is, from side to side. |
| `maximum` | `29999984` |  |
| `per-player` | `0` | How much wider the border is for each player on the network, on any of its servers. It widens as players come and narrows as they leave, never below what holds the islands. 0 sets the border by the islands alone. |
| `transition` | `30s` | How long the border takes to move to a new width. |
| `worlds` | empty list | The worlds the border is set in. Leave it empty for every world that only StrangerRealms presets make islands in. A world another preset shares is left alone, so no island is ever cut off. |
