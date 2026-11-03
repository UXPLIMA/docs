---
title: Shooting
order: 302
description: Every step from the release of the bow to the arrival, with the cooldown, the landing checks and the fall protection.
icon: move-up-right
---

## Step by step

| Step | What happens |
|---|---|
| 1. The release | The world filter and the cooldown are checked. Refused, no arrow leaves the bow and the refusal's effects run |
| 2. The shot | The arrow flies `arrow-speed-multiplier` times faster than a normal one, does `arrow-damage` damage, and is marked as this player's. The cooldown starts, the sweep shows on the bow, the `shot` effects run, and the trail starts |
| 3. The flight | An arrow that hits a player or a mob passes through it and flies on: only a block is a landing |
| 4. The landing | Where the arrow stopped is checked: the floor height, the room to stand, the region. The arrow is removed |
| 5. The departure | The region the player stands in now is checked. The `departure` effects run where they stand, the fall protection starts, and they are moved |
| 6. The arrival | The `arrival` effects run where they now stand |

The player arrives looking the way they were looking when they were moved, not the way the arrow flew.

## The teleport keys

| Key under `teleport` | Default | What it does |
|---|---|---|
| `cooldown-ms` | `3000` | Milliseconds between two shots; `0` turns the cooldown off |
| `fall-damage-grace-ms` | `6000` | Milliseconds after a teleport in which the first fall does no damage; `0` turns it off |
| `headroom-blocks` | `2` | Clear blocks the landing needs, counted upward from the block the arrow stopped in; at least `1`. `2` fits a standing player |
| `void-floor-y` | `-40.0` | The lowest Y a player may be sent to |
| `refund-cooldown-on-refusal` | `true` | Give the plugin's cooldown back when the refusal was only found after the arrow flew |
| `arrow-damage` | `0.0` | The damage the arrow does; `0` or more |
| `arrow-speed-multiplier` | `1.6` | How much faster than a normal arrow it leaves the bow; more than `0` |
| `arrow-allow-pickup` | `false` | Whether a player may pick the arrow up |

## The cooldown

The cooldown runs from the shot. It is kept twice: by the plugin, which refuses a shot inside it with
`refused-cooldown`, and on the bow, as the game's own item cooldown. The second draws the white sweep over the bow,
and the game does not let the bow be drawn while it runs. It belongs to this bow alone: another bow in the inventory
is not greyed.

`refund-cooldown-on-refusal` clears the plugin's cooldown after a landing or departure refusal. The sweep on the bow
is not cleared and runs to its end, so the player still waits for it before the next shot.

The cooldown is forgotten when the player leaves the server.

## The landing

| Check | Refused when | Effects |
|---|---|---|
| The floor | The arrow stopped below `void-floor-y` | `refused-void` |
| The room | Fewer than `headroom-blocks` passable blocks above the point the arrow stopped, counting that block | `refused-headroom` |
| The region | `filters.regions` closes the place the arrow landed | `refused-destination` |

A passable block is one a player can walk through: air, grass, flowers, water, an open door. A slab or a fence is
not. An arrow that falls out of the world, or leaves the loaded chunks, never lands: nothing happens and nothing is
said.

## The departure

When the landing passes, the plugin checks where the player stands now. `filters.regions` with `check` set to
`departure` or `both` refuses a player standing in a closed region with `refused-departure`. This is checked as they
go, so a player who walked into a closed region while the arrow flew is refused.

A player who left the server while the arrow flew is not moved.

## The fall protection

The protection starts just before the player is moved. For `fall-damage-grace-ms`, the first fall they take does no
damage, and the protection ends with that fall. A second fall, or a fall after the time, hurts as usual. Death, leaving
the server, or a teleport the server refused clears it.

Related: [Filters](filters.md), [Effects](effects.md), [config.conf](../config/config-conf.md#teleport)
