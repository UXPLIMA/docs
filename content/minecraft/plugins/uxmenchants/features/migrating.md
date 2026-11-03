---
title: Migrating
order: 313
description: Bringing another plugin's enchantments across with import, stripping ours from a world, and export.
icon: arrow-right-left
---

## import

`/uxmenchants import <lore|registry>` (`uxmenchants.admin.import`) reads what another product left on the items of
every online player, each on that player's own thread, and says how many were brought across a second later.

| Shape | Reads |
|---|---|
| `lore` | Plugins that write the enchantment as a lore line. A line that reads as one of our names, with or without a roman numeral, becomes that enchantment and the line comes off. Names are matched in every language the content was written in |
| `registry` | A plugin that registers its enchantments under `minecraft:`. A key whose name is one of our ids becomes that enchantment, held to its range, and the component comes off. Sharpness stays Sharpness |

Nothing is guessed: an item whose lines match nothing of ours is left as it was, so a run that finds nothing changes
nothing. Give your enchantments the same names or ids as the old plugin's, then run it. Items in chests and players who
are offline are not reached, so run it again as players come back. A word it does not know lists the two it does.

## strip

`/uxmenchants strip <world>` takes every enchantment of ours off the items of every player in that world. See
[Admin commands](../commands/admin.md#strip).

## export

`/uxmenchants export` prints what the held item carries as `id = level` lines.

## Leaving uxmEnchants

Nothing but an id and a level was ever written onto an item. Remove the plugin and those items keep an inert key and
lose our lines.

Related: [Admin commands](../commands/admin.md)
