---
title: Themes
order: 303
description: The blocks a course is built from, the picker icon, and writing your own.
icon: paintbrush
---

A theme is a list of blocks. Every block a course places is drawn from its list at random, so a theme of one block
builds a course all of one block, and a theme of five mixes them. A theme changes nothing about the jumps. Themes
live in the `themes` block of [`config.conf`](../config/config-conf.md#themes); the player menu calls them
palettes.

## The shipped themes

| Theme | Picker icon | Blocks |
|---|---|---|
| `stone` | Stone | Stone, cobblestone, andesite, smooth stone |
| `wood` | Oak planks | Oak, spruce, birch and dark oak planks |
| `quartz` | Quartz block | Quartz block, smooth quartz, quartz bricks, quartz pillar |
| `glow` | Sea lantern | Sea lantern, glowstone, shroomlight, ochre froglight |
| `wool` | Pink wool | Pink, light blue, lime, yellow and magenta wool |

`run.default-theme` (`stone`) is the theme of a player who has chosen none, in an arena whose own theme no longer
exists.

## The keys

```hocon
themes {
  ice {
    icon      = "PACKED_ICE"
    materials = ["PACKED_ICE", "BLUE_ICE", "SNOW_BLOCK"]
  }
}
```

| Key | Meaning |
|---|---|
| `icon` | The item that stands for the theme in the [picker](../menus/themes.md) |
| `materials` | The blocks drawn from, as Bukkit material names. At least one, or the reload stops with the theme's id |

The id on the left is what a player types after `/parkour theme`, what the picker shows as the name, and what an
arena names. Run `/uxmparkour reload` after a change.

## Choosing blocks

- **Full blocks only.** A player lands on the top face, so a slab, a stair, a fence or a carpet makes a jump
  shorter or taller than the generator measured. Use blocks a full cube tall.
- **Light.** In a dark sky, a theme with light blocks (`glow`) is the easiest to read.
- **Ice.** `ICE`, `PACKED_ICE` and `BLUE_ICE` are slippery, which makes every jump harder than its difficulty says.
- **Gravity blocks are safe.** Course blocks are placed without physics, so sand and gravel stay in the air.
- **A name the server does not know,** or one that is not a block, is placed as stone. Check a new theme in game.

## How a player picks one

| Way | What it does |
|---|---|
| `/parkour themes` | Opens the [picker](../menus/themes.md); a click chooses |
| `/parkour theme <theme>` | Chooses by name |
| Nothing | The arena's theme, then `run.default-theme` |

The choice is the player's in every arena and is kept in the database. A theme you removed from `config.conf` is
skipped for a player who had chosen it.

Related: [Difficulties](difficulties.md), [Theme picker](../menus/themes.md), [config.conf](../config/config-conf.md#themes)
