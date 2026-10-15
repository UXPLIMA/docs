---
title: Flags
order: 302
description: All 32 flags, what each permits, and which are on for a new claim.
icon: flag
---

A flag permits something when it is **present** in the claim and forbids it when it is absent. The owner
sets flags with `/claim setflag <flag> <true|false>`, flips one with `/claim toggleflag <flag>`, or uses the
*Flags* menu. Changing a flag also needs the ability node `uxmclaims.ability.claim.flag.<flag>` in lower
case, and waits out `uxmclaims.delay.flags` (5 seconds by default) between changes.

The **New** column is whether the flag is in `claimSettings.defaultFlags`, the set a new claim starts
with.

## Security and damage

| Flag | When present | New |
|---|---|---|
| `PVP` | Players may hurt each other: hits, projectiles, harmful splash and lingering potions, and end crystals, TNT or tamed animals set up by another player. Checked where the victim stands. | off |
| `KEEP_INVENTORY` | A player who dies here keeps items and levels, and drops nothing. | off |
| `CREEPER_DAMAGE` | Creeper explosions break blocks. | **on** |
| `WITHER_DAMAGE` | The wither and its skulls break blocks. | **on** |
| `FIRE_SPREAD` | Fire spreads, burns blocks away, and lava or fire ignites blocks. | **on** |
| `LIGHTNING_DAMAGE` | Lightning sets blocks on fire. | off |
| `MOB_GRIEFING` | Mobs without a flag of their own change blocks (sheep eating grass, ravagers, rabbits), and explosions without a flag of their own, a TNT minecart included, break blocks. Mobs and their arrows damage armor stands, item frames, paintings and vehicles. | off |

## Explosions

| Flag | When present | New |
|---|---|---|
| `TNT_EXPLOSIONS` | Primed TNT breaks blocks, and so do the block explosions the table below assigns to it. TNT that goes off in a claim without this flag breaks no block anywhere in its blast. | **on** |
| `END_CRYSTAL_DAMAGE` | End crystal explosions break blocks. | off |
| `BED_EXPLOSION` | Exploding beds break blocks. | off |
| `FIREBALL_DAMAGE` | Ghast fireballs break blocks. | off |
| `DRAGON_DAMAGE` | The ender dragon breaks blocks. | off |
| `RESPAWN_ANCHOR_EXPLOSION` | Exploding respawn anchors break blocks. Off in a new claim, so anchors leave its blocks alone. | off |

| Server | Flag of a block explosion |
|---|---|
| Paper 1.20.1 and later | Decided by the exploded block: a respawn anchor uses `RESPAWN_ANCHOR_EXPLOSION`, any bed `BED_EXPLOSION`, anything else `TNT_EXPLOSIONS`, such as an explosion another plugin creates at a location |
| Spigot and older Paper | Decided by the dimension: the Overworld uses `RESPAWN_ANCHOR_EXPLOSION`, the Nether and the End `BED_EXPLOSION` |

Primed TNT is an entity explosion, not a block explosion, and always uses `TNT_EXPLOSIONS`.

## World mechanics

| Flag | When present | New |
|---|---|---|
| `FLUID_FLOW` | Water and lava flow and change level. | **on** |
| `PISTON_PUSH` | Pistons inside the claim extend and retract. | **on** |
| `LEAF_DECAY` | Leaves decay. | off |
| `ICE_MELT` | Ice, packed ice, blue ice, snow and snow blocks melt. | off |
| `BLOCK_FADE` | Any other block fades, such as farmland drying back to dirt. Coral has its own flag; fire always burns out. | off |
| `BLOCK_FORM` | Blocks form from world conditions: snow and ice in cold weather, cobblestone, stone and obsidian where lava meets water, concrete powder setting. | **on** |
| `ENTITY_BLOCK_FORM` | Entities form blocks: a snow golem's trail, Frost Walker ice. | off |
| `STRUCTURE_GROW` | Saplings and mushrooms grow into trees and huge mushrooms. | **on** |
| `NATURE_SPREAD` | Blocks spread on their own: grass, mycelium, vines, small mushrooms. | **on** |

## Blocks

| Flag | When present | New |
|---|---|---|
| `CORAL_FADE` | Coral blocks and fans die out of water. | off |
| `SCULK_SPREAD` | Sculk spreads from a catalyst. | off |
| `REDSTONE` | Redstone carries power. Without it every redstone signal in the claim is forced to zero. | **on** |

## Mob spawning

These apply to natural spawns only. Spawners, spawn eggs, breeding and commands are never affected.

| Flag | When present | New |
|---|---|---|
| `MOB_SPAWNING` | The master switch: without it no mob spawns naturally. | **on** |
| `SPAWN_ANIMALS` | Animals spawn naturally. | **on** |
| `SPAWN_MONSTERS` | Hostile mobs spawn naturally, slimes, ghasts and magma cubes included. | **on** |
| `SPAWN_PHANTOMS` | Phantoms spawn. | off |

## Mob behaviour

| Flag | When present | New |
|---|---|---|
| `ENDERMAN_PICKUP` | Endermen pick up and place blocks. | off |
| `ZOMBIE_DOOR_BREAK` | Zombies break doors. | off |
| `SILVERFISH_INFEST` | Silverfish enter and leave infested blocks. | off |

## Notes

- **`MOB_SPAWNING` overrides the three spawn flags.** Without it nothing spawns naturally, whatever
  `SPAWN_ANIMALS`, `SPAWN_MONSTERS` and `SPAWN_PHANTOMS` say. Mobs that are neither animals nor hostile,
  such as squid, fish and bats, follow `MOB_SPAWNING` alone.
- **Explosion flags decide block damage, not entity damage.** With `TNT_EXPLOSIONS` off the TNT still
  goes off and still hurts whoever stands next to it; it only leaves the blocks alone. The claim block
  never breaks.
- **Villagers and bees are never griefing.** Villagers farming and bees growing crops or filling hives
  are allowed whatever `MOB_GRIEFING` says.
- **`BLOCK_FORM` off breaks cobblestone generators**, because lava meeting water forms no block.
- **`PVP` off applies inside the claim only.** A player can be chased out and killed one chunk away,
  which is why teleporting to a public warp in a claim with `PVP` on asks for confirmation.
- **`FIRE_SPREAD` and the `IGNITE` permission are different things.** `IGNITE` decides whether a player
  may start a fire; `FIRE_SPREAD` whether the fire travels. Allowing `IGNITE` but not `FIRE_SPREAD` gives
  you campfires that cannot burn the house down.
- **Cross-border movement is blocked whatever the flags say.** Fluids, fire, pistons and growth never
  cross a claim border; see [How protection works](overview.md).
- **Changing `defaultFlags` does not change existing claims.** It applies at creation only. Use
  `/claim admin bulk setflag <flag> <true|false>` to change every claim; read
  [Admin commands](../commands/admin.md) first.
