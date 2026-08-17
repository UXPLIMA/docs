---
title: Flags
order: 302
description: All 32 flags, what each permits, and which are on for a new claim.
icon: flag
---

A flag permits something when it is **present**. Removing it forbids that thing. Set them with
`/claim setflag <flag> <true|false>`, flip them with `/claim toggleflag <flag>`, or use the flags menu.

The **New** column is whether the flag is in `claimSettings.defaultFlags`: what a freshly created
claim gets.

## Security and damage

| Flag | When present | New |
|---|---|---|
| `PVP` | Players may damage each other inside the claim | off |
| `KEEP_INVENTORY` | Players keep their inventory when they die here | off |
| `CREEPER_DAMAGE` | Creeper explosions may break blocks | **on** |
| `WITHER_DAMAGE` | The wither may break blocks | **on** |
| `FIRE_SPREAD` | Fire may spread from block to block | **on** |
| `LIGHTNING_DAMAGE` | Lightning may set fires and damage entities | off |
| `MOB_GRIEFING` | Mobs may change blocks (endermen, sheep, villagers) | off |

## Explosions

| Flag | When present | New |
|---|---|---|
| `TNT_EXPLOSIONS` | TNT may break blocks | **on** |
| `END_CRYSTAL_DAMAGE` | End crystals may explode destructively | off |
| `BED_EXPLOSION` | Beds may explode in the Nether and the End | off |
| `FIREBALL_DAMAGE` | Ghast and blaze fireballs may break blocks | off |
| `DRAGON_DAMAGE` | The ender dragon may break blocks | off |
| `RESPAWN_ANCHOR_EXPLOSION` | Respawn anchors may explode outside the Nether | off |

## World mechanics

| Flag | When present | New |
|---|---|---|
| `FLUID_FLOW` | Water and lava may flow | **on** |
| `PISTON_PUSH` | Pistons may push and pull blocks | **on** |
| `LEAF_DECAY` | Leaves decay when the tree is cut | off |
| `ICE_MELT` | Ice and snow melt | off |
| `BLOCK_FADE` | Blocks fade (coral drying, farmland reverting, snow melting) | off |
| `BLOCK_FORM` | Blocks form (ice freezing, snow settling, concrete setting) | **on** |
| `ENTITY_BLOCK_FORM` | Entities form blocks (a snow golem's trail, frost walker ice) | off |
| `STRUCTURE_GROW` | Saplings, mushrooms and vines may grow into structures | **on** |
| `NATURE_SPREAD` | Grass, mycelium and vines may spread | **on** |

## Blocks

| Flag | When present | New |
|---|---|---|
| `CORAL_FADE` | Coral dies out of water | off |
| `SCULK_SPREAD` | Sculk may spread from a catalyst | off |
| `REDSTONE` | Redstone components may fire | **on** |

## Mob spawning

| Flag | When present | New |
|---|---|---|
| `MOB_SPAWNING` | The master switch (**off blocks every natural spawn**) | **on** |
| `SPAWN_ANIMALS` | Passive mobs may spawn naturally | **on** |
| `SPAWN_MONSTERS` | Hostile mobs may spawn naturally | **on** |
| `SPAWN_PHANTOMS` | Phantoms may spawn | off |

## Mob behaviour

| Flag | When present | New |
|---|---|---|
| `ENDERMAN_PICKUP` | Endermen may pick up blocks | off |
| `ZOMBIE_DOOR_BREAK` | Zombies may break down doors | off |
| `SILVERFISH_INFEST` | Silverfish may enter and leave infested blocks | off |

## Notes

- **`MOB_SPAWNING` overrides the three specific spawn flags.** When it is off, nothing spawns
  naturally, whatever `SPAWN_ANIMALS`, `SPAWN_MONSTERS` and `SPAWN_PHANTOMS` say. The listener checks
  the master switch first and returns. Leave it on and use the specific flags, unless you want a
  genuinely mob-free zone.

- **Explosion flags govern block damage, not entity damage.** `TNT_EXPLOSIONS` off means the TNT still
  goes off and still hurts whoever is standing next to it: it just does not break the base.

- **`PVP` off applies inside the claim only.** A player can be chased out of a claim and killed one
  chunk away. Warp confirmation exists for exactly this reason: teleporting to a public warp in a
  `PVP` claim prompts first.

- **`FIRE_SPREAD` and the `IGNITE` permission are different things.** `IGNITE` decides whether a
  player may put fire down. `FIRE_SPREAD` decides whether the fire that already exists travels. A
  claim that allows neither is genuinely fireproof; allowing `IGNITE` but not `FIRE_SPREAD` gives you
  decorative campfires that cannot burn the house down.

- **`REDSTONE` off stops the components firing**: clocks, dispensers, pistons wired to redstone. It
  is heavier than it looks, and it is on by default because turning it off breaks most farms.

- **Changing a default does not change existing claims.** `claimSettings.defaultFlags` only applies at
  creation. Use `/claim admin bulk setflag <flag> <value>` to change every loaded claim: read the
  warning on the [Admin commands](../commands/admin.md) page first.
