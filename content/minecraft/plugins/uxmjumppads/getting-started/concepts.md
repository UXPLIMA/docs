---
title: Concepts
order: 203
description: Matched pads and set pads, what a step reads, the cooldown, one throw per arrival, and the fall grace.
icon: lightbulb
---

Four ideas carry the whole plugin: the pad type, the two ways a block becomes a pad, the throw, and the landing.

## Two ways to be a pad

| | Matched pad | Set pad |
|---|---|---|
| Made by | Building: the type's `block` on the type's `under` | `/uxmjumppads set <type>` on the block you stand on |
| Where | Everywhere on the server the blocks match | That one block |
| Needs the type's blocks | yes | no: any block can be set |
| Throws toward | Where the stepping player faces | Where the operator faced when they set it |
| Kept in | nothing: the world is the list | `pads.conf` |

A set pad wins over a matched one on the same block.

## What a step reads

A step is read when the player's feet reach a new block. The plugin looks at three blocks: the one at the feet, the
floor under it, and the block under the floor.

| Pad block | Is | Under it |
|---|---|---|
| A plate, a carpet, a button | the block at the feet | the floor |
| A full block, such as slime | the floor | the block under the floor |

So a plate is walked into and a slime block is stood on, and one rule serves both. Standing still or turning on a
pad throws nobody.

A player who is flying, gliding, riding, or in spectator mode is never thrown.

## The throw

| Rule | Why |
|---|---|
| A player is thrown once per arrival | Coming back down on the pad that threw them, without standing anywhere else, does not throw again, so a straight-up pad does not juggle a player forever |
| `cooldown-ms`, 750, between two throws of one player | A player inside a plate makes several moves; without the wait each would throw |
| A pad with a `permission` throws only its holders | Anyone else walks over it as over any block, and is told nothing |

## The landing

After a throw, the first fall within `fall-damage.grace-ms` (10 seconds) costs no health. The grace is spent on that
fall: the next one is the player's own. A death or a quit drops it.

Related: [Your first pad](first-pad.md), [Pad types](../pads/types.md), [Throws and landings](../pads/throws.md)
