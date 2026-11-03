---
title: Fights
order: 303
description: The knockback stick, what a death inside an arena does, and how a kill is credited.
icon: swords
---

Getting the king off the plate is the game. The plugin hands every player the same weapon and keeps a death from
costing anything.

## The knockback stick

Every player who gets in is given the stick, set under `stick` in [config.conf](../config/config-conf.md#stick):

| Key | Default | Meaning |
|---|---|---|
| `material` | `"STICK"` | The item. A name the server does not know is drawn as a stick |
| `slot` | `0` | The hotbar slot, `0` to `8`. The stick is selected on entry |
| `knockback-level` | `2` | The Knockback level on it. `0` gives a plain item with no enchantment |

Its name and lore are the lines `stick.name` and `stick.lore` of the [messages files](../config/messages.md), drawn
in each viewer's own language: two players looking at the same stick each read it in theirs.

The stick goes to its slot when that slot is empty or holds the stick already, otherwise to the first free slot. It
never writes over an item. It is locked where it is: it cannot be clicked away, moved with a number key, dragged,
dropped, swapped to the off hand or placed, and it is not dropped on death. The plugin tells it from other items by
a mark of its own, never by its material, so a plain stick from a chest is just a stick.

<Callout type="warning" title="An arena that keeps the inventory leaves the stick behind">

An arena with `clear-inventory = false` gives the stick into the player's own inventory and takes nothing back on
leaving, so the player keeps a stick they cannot drop. Keep `clear-inventory` on for an arena whose players go back to
a lobby with items of their own.

</Callout>

## Deaths

A player who dies inside an arena loses nothing:

| What | Happens |
|---|---|
| Inventory | Kept, nothing dropped |
| Levels | Kept, no experience dropped |
| Respawn | At the arena's spawn, not the world spawn |
| Session | Still in the arena |
| Crown | A king who dies loses the crown; the reign ends and is recorded |

The `fell` moment runs for the player who died, whether or not anybody killed them.

## Kills

A death is a kill when the server names a killer and the killer is in the same arena as the victim. The server names
a killer for a player who dies after a hit from another player, a fall after a hit included, within the game's own
time window.

| Counted | Not counted |
|---|---|
| A player in the arena who knocks another off the tower to their death | A death with no player behind it |
| | A killer outside the arena, or in another arena |
| | A player who kills themselves |

A kill adds one to the killer's kills and to the `kills` board, pays the kill reward
([Rewards](rewards.md)), runs `fell` for the victim and `knocked-off` for the killer with `<player>` as the
victim's name. Pushing the king off the plate without killing them is not a kill: it ends their reign and nothing more.

## Notes

- **Damage is the server's.** The plugin turns nothing off: fall damage, hunger and the hits themselves follow the
  world's rules and the arena's game mode. Adventure mode, the default, keeps the tower from being broken.
- **No protection plugin needed for the items.** The stick's lock is the plugin's own.

Related: [The crown](crown.md), [Rewards](rewards.md), [Moments](moments.md)
