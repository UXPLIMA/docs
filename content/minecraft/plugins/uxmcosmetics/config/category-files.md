---
title: Category files
order: 702
description: Every key a piece takes in categories/<family>.conf, common and by kind.
icon: folder-tree
---

One file per family under `categories/`, each holding a `cosmetics = [ ... ]` list. Every piece is data: add one,
reload, and it is in the menu. The comments at the top of each file list its keys.

## Every piece

| Key | Meaning |
|---|---|
| `id` | Lower case letters, digits, hyphen and underscore. Stored in the database and part of the node: never change it once players own it |
| `name` | What a player reads, when the messages files have no `cosmetic.<family>.<id>.name` |
| `description` | One sentence under the name; without one, the family's shared sentence |
| `rarity` | `common`, `uncommon`, `rare`, `epic` or `legendary` |
| `price` | What a purchase costs; `0` is not for sale |
| `permission` | The node; default `uxmcosmetics.<family>.<id>` |
| `icon` | The menu item: a material, or a block with `material`, `color`, `custom-model-data` and `skull-texture` |

## Particle families

Trails, halos and cloaks.

| Key | Meaning |
|---|---|
| `particle` | `{ type, color-from, color-to, size, count, spread, speed }`. `color-from = "rainbow"` walks the hue circle |
| `pattern` | `TRAIL`, `HALO`, `CLOAK`, `WINGS`, `ORBIT` or `HELIX` |
| `radius` | How wide |
| `points` | How many points or rows |
| `height` | Above the feet |
| `period-ms` | How often a frame is drawn, rounded up to whole animation ticks |

## Entity families

Pets, balloons, mounts and morphs.

| Key | Meaning |
|---|---|
| `entity` | An entity type: `WOLF`, `CREEPER`, `SKELETON_HORSE`; for a balloon `ITEM_DISPLAY` or `BLOCK_DISPLAY` |
| `attachment` | `FOLLOW`, `FLOAT`, `RIDE` or `DISGUISE`; the family's own by default |
| `offset` | `{ side, up, back }` in blocks from the player |
| `baby` | Spawn the small form |
| `glowing` | An outline everyone sees |
| `item`, `scale`, `lead` | A balloon's block, its size, and whether the lead is drawn |
| `dress` | A mount's `color`, `style`, `armor` and `rainbow` |
| `aura` | A particle drawn at a mount every frame |

## Worn families

Hats, suits and banners.

| Key | Meaning |
|---|---|
| `item` | The head item: a material, or a block with `material`, `color`, `custom-model-data`, `skull-texture`, and for a banner `patterns` |
| `pieces` | Several slots: `head`, `chest`, `legs`, `feet`, `off-hand`, each a material or an item block |

`color` is `#rrggbb` on leather, or `"rainbow"`. A banner's `patterns` is a list of `{ pattern, color }`.

## Burst families

Emotes and gadgets.

| Key | Meaning |
|---|---|
| `steps` | `{ particle {...}, figure, radius, height, delay-ms }`; `figure` is `POINT`, `RING`, `SPHERE` or `CONE` |
| `sound`, `volume`, `pitch` | The sound |
| `cooldown-ms` | The wait between two uses |
| `bubble` | An emote's face: `{ item, ms, height }` |
| `item` | A gadget's hotbar item |
| `action` | A gadget's behaviour; keep the one a shipped gadget names |
| `push` | A gadget's throw: `{ forward, up }` in blocks per tick |

## Names per language

A shipped piece's name and sentence are in every messages file under `cosmetic.<family>.<id>.name` and
`.description`. A piece you add reads them from its entry until you add those lines.

Related: [Catalogue](../catalogue/overview.md), [Particles](../features/particles.md), [Followers](../features/followers.md)
