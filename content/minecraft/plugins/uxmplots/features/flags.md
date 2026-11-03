---
title: Flags
order: 404
description: The thirty four flags, world defaults, locked flags, and what levels unlock.
icon: toggle-left
---

A flag is a switch on one plot. `/plot flag <flag> [value]` reads or sets one; `/plot flags` lists them; the
[flags window](../menus/panel.md#flags) shows them as levers (left click turns one over, right click lets the world
decide again).

| Flags | |
|---|---|
| Building | `break`, `place`, `tnt-placing` |
| Fighting | `pvp`, `pve`, `hostile-damage` |
| World | `explosions`, `fire-spread`, `ice-form`, `snow-form`, `grass-grow`, `crop-grow`, `vine-grow`, `leaf-decay`, `water-flow`, `lava-flow`, `mob-spawning` |
| Use | `vehicle-use`, `redstone`, `pressure-plate`, `door`, `lever`, `buttons`, `item-frame-use`, `armour-stand-use`, `portal-use` |
| Items | `item-drop`, `item-pickup` |
| Player | `entry`, `hunger`, `fall-damage`, `fly` |
| Plot | `chat` (the people standing here have a channel of their own), `hologram` (the plot wears its sign) |

The shipped values are chosen for a creative world. Off: `pvp`, `explosions`, `fire-spread`, `mob-spawning`,
`hostile-damage`, `hunger` and `fall-damage`. Everything else is on, `fly` included, because building from the air is
most of how a plot is built.

## World defaults and locks

```hocon
flags {
  mob-spawning = true
}
locked-flags = ["pvp"]
```

In a world file, `flags` writes only the values you disagree with. A flag in `locked-flags` answers with the world's
value and the plot's own setting is ignored. A plot at a [level](levels.md) that unlocks a flag may set it even where
the world locks it.

Related: [Plot worlds](worlds.md)
