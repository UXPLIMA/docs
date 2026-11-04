---
title: Actions
order: 1070
description: Every action a rule can name, what in the game each one covers, and how the tree works.
icon: list-tree
---

Contents: [How the tree works](#how-the-tree-works) · [Building](#building) · [Using blocks](#using-blocks) ·
[Entities](#entities) · [Damage](#damage) · [Items](#items) · [Vehicles, hangings, trampling](#vehicles-hangings-trampling) ·
[Movement](#movement) · [Nature](#nature)

## How the tree works

An action is a dotted name such as `use.door`. The part before a dot is its parent, so `use` is the parent of
`use.door`, `use.container` and every other `use.*` action. A rule on a parent covers every child that has no
rule of its own:

```text
use everyone deny
use.door everyone allow
```

Here doors open and nothing else does. `/guard actions` prints the top level and `/guard actions use` every
action under `use`. Other plugins can add their own branch through the [developer API](../developer/queries.md),
and their actions are then written in rules like these.

A block, item or entity that no table below names is never let through by accident: a right click on an
unlisted block is `use.other`, on an unlisted entity `interact.other`, and so on.

Player actions are decided for the player who does them. Actions under `world.*` have no player behind them:
only `everyone` rules count for them, plus the [border test](concepts.md#parents-and-families). A nature action a
player starts (lighting a fire, priming TNT, emptying a lava bucket) is checked both ways.

## Building

| Action | Covers |
|---|---|
| `break` | Breaking a block, and a block taken by an entity a player controls |
| `place` | Placing a block, and tools that reshape one: axes stripping logs, shovels making paths, hoes tilling, honeycomb waxing, brushes, glass bottles, shears |

## Using blocks

| Action | Covers |
|---|---|
| `use` | Parent of every right click on a block |
| `use.door` | Doors |
| `use.trapdoor` | Trapdoors |
| `use.gate` | Fence gates |
| `use.button` | Buttons |
| `use.lever` | Levers |
| `use.pressure-plate` | Stepping on pressure plates. Quiet: a player standing on one is not told every tick |
| `use.container` | Chests, barrels, shulker boxes, furnaces, hoppers, droppers, dispensers, brewing stands, chiseled bookshelves, decorated pots |
| `use.workstation` | Crafting table, crafter, anvils, enchanting table, smithing table, loom, grindstone, stonecutter, cartography and fletching tables |
| `use.bed` | Beds |
| `use.respawn-anchor` | Respawn anchors |
| `use.redstone` | Repeaters, comparators, daylight detectors |
| `use.sign` | Editing signs |
| `use.lectern` | Lecterns |
| `use.cake` | Cakes and candle cakes |
| `use.beacon` | Beacons |
| `use.jukebox` | Jukeboxes and note blocks |
| `use.composter` | Composters |
| `use.cauldron` | Cauldrons |
| `use.bell` | Bells |
| `use.other` | Any other block with a right click, the ender chest included |

## Entities

| Action | Covers |
|---|---|
| `interact` | Parent of every right click on an entity |
| `interact.villager` | Villagers and wandering traders (trading) |
| `interact.animal` | Animals, fish and bats (feeding, breeding, shearing, leashing, milking) |
| `interact.armor-stand` | Taking from or putting on an armor stand |
| `interact.item-frame` | Putting in, taking out or turning an item in a frame |
| `interact.vehicle` | Getting into boats and minecarts, riding |
| `interact.other` | Any other entity |

## Damage

| Action | Covers |
|---|---|
| `damage` | Parent of every hit by a player, their arrows and potions included |
| `damage.player` | Hurting another player |
| `damage.animal` | Hurting animals, fish and bats |
| `damage.monster` | Hurting monsters |
| `damage.villager` | Hurting villagers and wandering traders |
| `damage.armor-stand` | Hitting armor stands |
| `damage.hanging` | Hitting item frames and paintings |
| `damage.vehicle` | Hitting boats and minecarts |
| `damage.other` | Hurting any other entity |

## Items

| Action | Covers |
|---|---|
| `item` | Parent of every item action |
| `item.drop` | Dropping an item |
| `item.pickup` | Picking an item up. A refused pickup is quiet: no message, no alert |
| `item.use` | Parent of using an item in hand |
| `item.use.ender-pearl` | Throwing ender pearls |
| `item.use.chorus-fruit` | Teleporting by chorus fruit |
| `item.use.firework` | Launching fireworks |
| `item.use.potion` | Throwing splash and lingering potions |
| `item.use.wind-charge` | Throwing wind charges |
| `item.use.bucket` | Filling or emptying any bucket |
| `item.use.spawn-egg` | Spawn eggs |
| `item.use.bone-meal` | Bone meal |
| `item.use.lighter` | Flint and steel, fire charges |
| `item.use.other` | Throwing anything else: snowballs, eggs, tridents |

## Vehicles, hangings, trampling

| Action | Covers |
|---|---|
| `vehicle.place` | Placing boats and minecarts |
| `vehicle.destroy` | Breaking boats and minecarts |
| `hanging.place` | Hanging item frames and paintings |
| `hanging.break` | Taking down item frames and paintings |
| `trample.crop` | Trampling farmland, by a player (quiet, like a pickup) or by a mob (checked as nature) |
| `trample.turtle-egg` | Stepping on turtle and sniffer eggs. Quiet |
| `trample.dripleaf` | Tipping big dripleaf. Quiet |

`vehicle`, `hanging` and `trample` are parents too, so `trample everyone deny` covers all three kinds.

## Movement

| Action | Covers |
|---|---|
| `move.enter` | Walking, riding, flying or teleporting into the region |
| `move.exit` | Leaving it the same ways |
| `move.teleport-in` | Arriving by teleport only: commands, plugins, ender pearls, chorus fruit, portals |
| `move.teleport-out` | Leaving by teleport only |

A refused walk pushes the player back to where they last stood outside (set `enter.push-back = false` to only
stop them). Every teleport, from `/home`, `/warp` and `/tpa` to any other plugin, is checked against both
`move.enter` and `move.teleport-in`.

## Nature

Only `everyone` rules count here. Leave these unset and they cost nothing.

| Action | Covers |
|---|---|
| `world.explosion.creeper` | Creeper explosions |
| `world.explosion.tnt` | TNT and TNT minecarts |
| `world.explosion.wither` | The wither and its skulls |
| `world.explosion.ghast` | Ghast fireballs and other fireballs |
| `world.explosion.bed` | Beds and respawn anchors exploding outside their dimension |
| `world.explosion.crystal` | End crystals |
| `world.explosion.breeze` | Wind charges bursting |
| `world.explosion.other` | Any other explosion |
| `world.fire.spread` | Fire spreading |
| `world.fire.burn` | Fire burning blocks away, and fire going out |
| `world.fire.lava` | Lava setting blocks alight |
| `world.fire.lightning` | Lightning setting blocks alight |
| `world.liquid.water` | Water flowing |
| `world.liquid.lava` | Lava flowing |
| `world.piston` | Pistons pushing and pulling |
| `world.dispenser` | Dispensers firing |
| `world.hopper` | Hoppers moving items |
| `world.grow.tree` | Saplings and fungi growing |
| `world.grow.crop` | Crops growing |
| `world.grow.vine` | Vines, cave vines, weeping and twisting vines, glow lichen spreading |
| `world.grow.sculk` | Sculk spreading |
| `world.grow.amethyst` | Amethyst buds growing |
| `world.grow.other` | Bamboo, chorus, kelp, sugar cane and cactus growing |
| `world.spread` | Anything else spreading: grass, mycelium, moss, mushrooms |
| `world.decay` | Leaves decaying |
| `world.form` | Blocks forming on their own: snow, ice, frosted ice, concrete powder hardening, lava turning to stone, cobblestone or obsidian |
| `world.melt` | Snow, snow blocks, ice and frosted ice melting |
| `world.fade` | Other blocks fading on their own, such as coral dying, and farmland drying out |
| `world.mob.enderman` | Endermen taking or placing blocks |
| `world.mob.ravager` | Ravagers breaking crops and leaves |
| `world.mob.wither` | The wither breaking blocks as it moves |
| `world.mob.silverfish` | Silverfish entering blocks |
| `world.mob.zombie-door` | Zombies breaking doors |
| `world.mob.snow-golem` | Snow golem trails |
| `world.mob.other` | Any other mob changing a block (sheep eating grass, foxes taking berries) and mobs breaking item frames and paintings |
| `world.portal` | Portals forming |
| `world.dragon-egg` | The dragon egg teleporting |
| `world.lightning` | Lightning striking |

High-frequency events (liquids, fire spread, growth, decay, forming, melting) can be skipped for a whole world
with `worlds.<world>.high-frequency = false` in the config, for a world where those rules do not matter.
