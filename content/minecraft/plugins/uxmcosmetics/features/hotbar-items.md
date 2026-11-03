---
title: Hotbar items
order: 405
description: The wardrobe opener, the gadget slot, and the rules every cosmetic item follows.
icon: hand
---

## The opener

A nether star in hotbar slot 8 opens the wardrobe on a right click.

| Key in `opener` | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Hand out the opener at all |
| `give-on-join` | `true` | Put it in the hotbar at every join |
| `slot` | `8` | The hotbar slot, 0 to 8 |
| `material` | `NETHER_STAR` | The item |
| `keep-on-death` | `true` | Hand it back after a respawn |
| `colour` | `"3"` | The title's colour: a place on the theme's wheel, a role or a gradient |

Its words are `opener.name`, `crumb`, `description` and `lore` in the messages files, in each player's language. When
several plugins hand out hotbar items, give each a different slot and colour.

## The gadget slot

A worn gadget is put in `gadget.slot` (5). Its item carries the gadget's name and what a click does, and a white bar
sweeps across it while its cooldown runs, as over an ender pearl.

## Rules for every cosmetic item

Hats, suits, banners, gadgets and the opener carry the plugin's mark, and a marked item:

- cannot be moved, shift clicked, number-keyed, dragged, dropped, swapped to the other hand or placed;
- never drops on death;
- is taken back the moment the piece is taken off or the player leaves.

The armour a player really wore is put back when a hat or suit comes off.

Related: [config.conf](../config/config-conf.md#opener), [Emotes and gadgets](emotes-and-gadgets.md)
