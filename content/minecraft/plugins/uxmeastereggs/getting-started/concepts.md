---
title: Concepts
order: 103
description: Eggs and types, what a find counts, repeat finds, milestones and completion, and the board.
icon: lightbulb
---

Five ideas carry the whole plugin: the egg, the type, the find, the milestone and the board.

## Eggs

An egg is a position: a world and a block. The plugin stores it in the database with a random id and the name of its
type, and leaves the block itself as you built it. Any block can be an egg; a player head with a texture is the usual
choice, because it looks like one.

| An egg has | Notes |
|---|---|
| An id | A UUID. Commands accept its first eight characters, or any start of it no other egg shares |
| A type | The name of a file in `types/` |
| A position | One block. One egg per block |

The map's total is the number of eggs placed, whatever their type. Removing an egg removes every find of it, so
nobody keeps a find of an egg that is gone.

## Types

A type is a file in `types/`, and the file name is its name: `types/golden.conf` is the type `golden`. It says:

| Part | What it is |
|---|---|
| `display-name` | The name players read, usually an `@key` of the messages files |
| `texture` | The head drawn for it in the menus |
| `cooldown` | Empty for once only; a time such as `1d` makes it findable again |
| `reward` | Money lines and console commands paid at each find |
| `particle` | The idle particle drawn on its eggs |
| `effects` | What the finder sees and hears |

See [Types](../hunting/types.md).

## Finds

A player clicks an egg, left or right:

| The player has | Type | Outcome |
|---|---|---|
| Never found this egg | any | A first find: the type's reward and effects, one more on the count and the board, milestones and completion checked |
| Found it, no cooldown | once only | *You have already found this egg.* Nothing else |
| Found it, inside the cooldown | repeatable | *This egg is ready again in 5h 12m.* |
| Found it, cooldown over | repeatable | A repeat find: the type's reward and effects again. The count and the board do not move |

A click on an egg never breaks or uses the block. Finding needs no permission.

## Milestones and completion

A milestone pays when a first find brings the player's count to its number or past it; each pays once per player.
Completion pays when a first find brings the count to the map's total. Eggs placed after a player completed the
map raise the total, and finding the new ones completes it, and pays, again. Both are in `config.conf`.
See [Rewards](../hunting/rewards.md).

## The board

| Board | Ranks by |
|---|---|
| `found` | First finds in the period |

It is kept for `alltime`, `monthly`, `weekly` and `daily`, read again every `leaderboard.refresh-seconds` (15), and
answered from memory. A repeat find moves no board. See [Placeholders](../placeholders/reference.md).

Related: [Your first hunt](first-hunt.md), [Eggs and finds](../hunting/overview.md), [Types](../hunting/types.md)
