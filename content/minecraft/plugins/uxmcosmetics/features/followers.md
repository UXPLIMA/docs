---
title: Followers
order: 307
description: Pets, balloons, mounts and morphs, how each is moved, and what keeps a cosmetic mob a costume.
icon: dog
---

Pets, balloons, mounts and morphs are entities moved by the shared animation task, never by their own brain.

| Family | Attachment | What it is |
|---|---|---|
| Pets | `FOLLOW` | An entity walking behind the player at `offset` |
| Balloons | `FLOAT` | A block display over the shoulder, on a lead |
| Mounts | `RIDE` | An entity the player sits on and steers |
| Morphs | `DISGUISE` | The player is invisible and an entity stands where they are |

## Pets

Silent, invulnerable, without its own intelligence, and it cannot be ridden or pushed. `baby = true` spawns the small
form, `glowing = true` adds an outline everyone sees. `animation.follower-speed` (0.35) is the share of the distance a
follower travels each frame: 1.0 snaps, a low value trails behind.

## Balloons

Two entities: a block or item display, and an invisible baby chicken with no brain kept under it to hold the lead,
because vanilla only ties a lead to a mob. `entity` is `ITEM_DISPLAY` or `BLOCK_DISPLAY`, `item` the block, `scale`
its size (about 0.7), `offset` side, up and back from the player, `lead` whether the string is drawn.

## Mounts

A horse, donkey, mule, camel, skeleton horse or zombie horse is tamed and saddled, and vanilla lets the rider steer
it. Any other animal is steered by the plugin from the keys the rider holds. Avoid a pig or a strider: vanilla gives
it a control item that fights the plugin.

Getting off takes the mount off: pressing shift unequips it. The mount is removed when it is taken off, the player
leaves, or the plugin stops.

| Key | Meaning |
|---|---|
| `dress.color` | Horse colour, or a dye colour for anything dyed |
| `dress.style` | A horse's markings |
| `dress.armor` | Horse armour, by material |
| `dress.rainbow` | The colour walks the dye circle |
| `aura` | A particle drawn at the mount every frame |

## Morphs

The player is made invisible and an entity stands in their place. Others see the entity; the wearer sees it in third
person; their own name stays above it. One entity per wearer, the most expensive family.

## A costume, not a mob

A mob with the plugin's mark never burns in the sun, turns into a drowned, converts in the overworld, lays eggs,
splits, teleports away, picks up blocks, hunts players, trades or is sheared. It is never saved to the world: a
follower whose owner is not online is removed at start (`cleanup.sweep-on-start`) and by `/uxmcosmetics cleanup`.

Related: [Category files](../config/category-files.md#entity-families), [Catalogue](../catalogue/pet.md)
