---
title: Turning PvP on and off
order: 301
description: The toggle item, the warm-ups in and out, who may hit whom, and the glow and hit particles.
icon: toggle-right
---

A player fights only after asking to. They press the toggle item or type `/pvp`, stand still through a warm-up, and
are in. The same press takes them out again.

## The warm-ups

| Direction | Setting | Shipped | While it runs |
|---|---|---|---|
| In | `warm-up.enter-ms` | 3000 | The player is still out: nobody can hit them and they cannot hit |
| Out | `warm-up.leave-ms` | 8000 | The player is still in: they can hit and be hit, and they keep the kit |

| During a warm-up | What happens |
|---|---|
| The player moves further than `warm-up.move-limit` (1.5 blocks) from where they pressed | It stops; `warm-up-moved` runs |
| The player presses again | It stops; `warm-up-cancelled` runs |
| The player dies or quits | It stops silently |
| It runs out | The switch happens: `pvp-on` or `pvp-off` runs |

With `warm-up.show-action-bar` on, the action bar counts down every second: *PvP starts in 3*, *PvP stops in 8*. A
warm-up set to `0` switches at once. The way out is longer on purpose: a player who is losing cannot step out the
moment the fight turns, and the move limit keeps them in reach.

## Who may hit whom

With `arena.only-fighters-may-fight` on, as shipped, a hit between two players lands only when both are fighting.
A player warming up to leave still counts as fighting; one warming up to join does not yet.

| Attacker | Victim | The hit |
|---|---|---|
| Fighting | Fighting | Lands, and tags both ([Combat tag](combat-tag.md)) |
| Not fighting | anyone | Refused: *Turn PvP on before you fight.* |
| Fighting | Not fighting | Refused: *Steve does not have PvP on.* |

A refusal line is said at most once every three seconds per attacker, so a player swinging at a crowd is not flooded.
Arrows and other projectiles count as hits from the player who shot them. A player hitting themselves is ignored.

With the setting off, every hit lands as the world's own rules say, and a hit between two fighters still tags them.
Turn it off only on a world whose own PvP rules decide who may fight. Hits inside another plugin's game can be left
to that game: see [Other games](../integrations/other-games.md).

## The toggle item

Every player is given the toggle item on join: an iron sword named *PvP* in hotbar slot 2. A click with it, left or
right, at the air or a block, presses the toggle; the click does nothing else. While a player fights, the kit takes
their hotbar, and the item comes back on top of it as a glowing red dye named *Leave PvP*, in slot 8.

| Key under `toggle-item` | Shipped | What it does |
|---|---|---|
| `enabled` | `true` | `false` hands out no item, on join or on top of the kit. An item a player already holds still works, and so does `/pvp` |
| `give-on-join` | `true` | Puts the item in the player's inventory on every join |
| `slot` | `2` | The hotbar slot, `0` to `8` |
| `available-in-pvp` | `true` | Puts the leave face on top of the kit, so a fighter is never without a way out |
| `item` | iron sword | The lobby face. See [config.conf](../config/config-conf.md#toggle-item) |
| `in-pvp.slot` | `8` | The slot of the leave face |
| `in-pvp.item` | red dye | The leave face |

The item goes to its slot when that slot is empty or holds the item already, otherwise to the first free slot; it
never writes over another item. It is locked: it cannot be dropped, moved, swapped to the off hand or placed, and it
is not dropped on death. Its name and lore are drawn in each viewer's language.

The leave face is set into its slot over whatever the kit put there, so keep `in-pvp.slot` clear of the kit's extras.
Delete the `in-pvp` block and the lobby face comes back at its own slot.

## The glow and the hit particles

| Key under `visuals` | Shipped | What it does |
|---|---|---|
| `glow` | `true` | Outlines every fighter, so the lobby can see who is in. The colour is the player's scoreboard team colour, which uxmGlow sets; without it the outline is white. A player who already glows is left alone |
| `hit-particles` | `true` | Draws particles on a fighter who is hit |
| `hit-particle` | `"CRIT"` | The particle, with its data after a space: `"dust #ff0000"`, `"block oak_log"`, `"dust_color_transition #ff0000>#0000ff"`. A name the server does not know draws `CRIT` |
| `hit-particle-count` | `12` | How many, at least 1 |

## Notes

- **The toggle item checks no node.** `uxmhubpvp.use` hides `/pvp`, but a player holding the item can still press
  it. Keep the item away from a group that should not fight.
- **Teleports do not stop a fight.** A fighter who is teleported keeps fighting; only the move limit of a warm-up
  watches where a player goes.

Related: [The kit](kit.md), [Combat tag](combat-tag.md), [Moments](moments.md)
