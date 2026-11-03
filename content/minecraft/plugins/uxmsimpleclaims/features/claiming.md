---
title: Claiming
order: 402
description: Claiming, growing, giving up, merging, splitting, names, the map and borders.
icon: map
---

## Claiming

- `/claim` claims the chunk you stand in, or opens the menu of the claim you stand in.
- A chunk that touches one of your claims joins it; one apart starts a new claim, named `claim1`, `claim2` and on
  unless you name it with `/claim here <name>`. A claim is always one connected piece.
- `/claim radius <n>` claims the square around you, `n` chunks to each side, up to `claims.max-radius` (5).
- `/claim auto` claims as you walk, `/claim auto unclaim` gives land up as you walk; run it again to stop.
- A free chunk on the [map](../menus/claims.md#map) is claimed with a click.

Claims keep `claims.min-distance` (1) chunks away from other players' claims and never cover a WorldGuard region.

## Giving up

`/claim unclaim` gives up the chunk you stand in, unless that would cut the claim in two. The last chunk deletes the
claim and asks again. `/claim delete` deletes the whole claim after you type it again within ten seconds. Part of the
price comes back (`economy.refund-percent`, 50). A deleted claim is kept as a [snapshot](staff-tools.md#snapshots).

## Merging and splitting

`/claim merge <claim>` joins another of your claims that touches this one. `/claim split <name>` cuts off the side of
the claim you face, the row you stand in included, as a new claim.

## A new claim

| Key in `claims` | Default |
|---|---|
| `default-icon` | `GRASS_BLOCK` |
| `default-colour` | `#4caf50` |
| `default-category` | `home` |
| `categories` | `home`, `shop`, `farm`, `town`, `build`, `other` |
| `name-min`, `name-max` | 3, 24 |
| `text-max` | 64, for a description, greeting or farewell |

## Seeing land

`/claim map` opens the land around you as a window, one cell a chunk. `/claim border` and the claim window draw the
edges for `display.border-seconds` (10), as `particles` or `blocks` (glowing block displays only the viewer sees).

Related: [Land commands](../commands/land.md), [Prices and limits](prices-and-limits.md)
