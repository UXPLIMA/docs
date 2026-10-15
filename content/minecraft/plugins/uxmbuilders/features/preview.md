---
title: Preview
order: 202
description: The structure drawn in the player's client before anything is placed.
icon: scan-eye
---

Right-clicking with a blueprint opens a preview. The player sees where the structure will stand, in which
direction it faces, and whether it is allowed there, before any money or blocks change hands.

## Two layers

| Layer | What it is |
|---|---|
| **Silhouette** | The structure's outer blocks sent to that player's client as fake block changes. No block is placed and no entity spawns; other players see nothing |
| **Outline** | A particle box around the footprint and an arrow on the front face. Green where allowed, red where not |

`preview.mode: BOX` keeps only the outline. A schematic with more surface blocks than
`preview.silhouetteMaxBlocks` falls back to the outline on its own, which guards the player's bandwidth.

## Controls

| Input | Does |
|---|---|
| Right-click (first time) | Open the preview |
| Left-click | Rotate by 90 degrees |
| Shift + left-click | Mirror |
| Shift + scroll one step | Shift the height by one block, up to 5 |
| `/builders rotate`, `mirror`, `offset <x> <y> <z>` | The same from chat |
| Right-click | Confirm and start the build |

The preview follows the block the player looks at, up to `preview.maxDistance` blocks away, and sits on
top of it, at it, or below it according to `start.origin.yMode`.

## Why it turns red

The colour is the same check the build runs when it starts, so a green preview is never refused.

| Reason | Message |
|---|---|
| Land protection denies building | `errorRegionDenied`, naming the plugin and the area |
| The world is not allowed | `errorWorldNotAllowed` |
| The build leaves the height limits | `errorHeightLimit` |
| Too close to spawn | `errorTooCloseToSpawn` |

The reason is said once when the preview turns red, not on every refresh. The check runs again at the
moment of confirming, so a spot that just turned red cannot slip through.

## When it closes

The preview closes when the blueprint leaves the main hand: switching slots, dropping it, moving it in
the inventory, swapping hands, or dying. Holding another blueprint switches the preview to that package.

`/builders preview <package>` opens a preview without a blueprint, so a player can see a package on the
ground before buying it. `/builders preview` with no argument closes it.

## Settings

| Key | Default | Meaning |
|---|---|---|
| `preview.enabled` | `true` | Off means a blueprint right-click builds where the player looks, facing away from them |
| `preview.mode` | `SILHOUETTE` | `SILHOUETTE` or `BOX` |
| `preview.refreshTicks` | `10` | How often the colour and outline refresh |
| `preview.maxDistance` | `64` | How far away the player may aim |
| `preview.silhouetteMaxBlocks` | `20000` | Above this, the outline only |
| `preview.colors.*` | green, red, yellow | `valid`, `invalid` and `direction` as hex |

Related: [Placement rules](placement-rules.md), [Land protection](protection.md)
