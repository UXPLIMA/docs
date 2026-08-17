---
title: Flags
order: 405
description: Turning the claim-wide rules on and off.
icon: flag
---

`menu/claim_flags.yml`, opened from claim management.

Every one of the 32 [flags](../protection/flags.md) appears as an item showing its current state.
Clicking toggles it.

A flag being **on** means the thing is permitted. Turning `FIRE_SPREAD` off stops fire spreading;
turning it on lets it spread again.

## What each toggle needs

`uxmclaims.ability.claim.flag.<FLAG>` for that specific flag, or `…flag.*` for all of them. Flags a
player may not change are shown but refuse on click.

This is per-flag on purpose. Granting `uxmclaims.ability.claim.flag.KEEP_INVENTORY` and
`…flag.MOB_SPAWNING` lets players tune the things that affect only them, while PvP stays a server
decision.

## The groups on screen

| Group | Flags |
|---|---|
| Security and damage | `PVP`, `KEEP_INVENTORY`, `CREEPER_DAMAGE`, `WITHER_DAMAGE`, `FIRE_SPREAD`, `LIGHTNING_DAMAGE`, `MOB_GRIEFING` |
| Explosions | `TNT_EXPLOSIONS`, `END_CRYSTAL_DAMAGE`, `BED_EXPLOSION`, `FIREBALL_DAMAGE`, `DRAGON_DAMAGE`, `RESPAWN_ANCHOR_EXPLOSION` |
| World mechanics | `FLUID_FLOW`, `PISTON_PUSH`, `LEAF_DECAY`, `ICE_MELT`, `BLOCK_FADE`, `BLOCK_FORM`, `ENTITY_BLOCK_FORM`, `STRUCTURE_GROW`, `NATURE_SPREAD` |
| Blocks | `CORAL_FADE`, `SCULK_SPREAD`, `REDSTONE` |
| Spawning | `MOB_SPAWNING`, `SPAWN_ANIMALS`, `SPAWN_MONSTERS`, `SPAWN_PHANTOMS` |
| Mob behaviour | `ENDERMAN_PICKUP`, `ZOMBIE_DOOR_BREAK`, `SILVERFISH_INFEST` |

## Two that surprise people

- **`MOB_SPAWNING` is a master switch.** Off, nothing spawns naturally regardless of the three
  specific spawn flags. A player turning it off to stop creepers also stops their cow farm.
- **`REDSTONE` off breaks farms.** It stops redstone components firing entirely, which is heavier than
  most players expect from the name.

The state text comes from `statusActive` and `statusPassive` in `messages.yml`.
