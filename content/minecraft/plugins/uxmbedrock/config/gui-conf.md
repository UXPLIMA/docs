---
title: gui.conf
order: 605
description: Menu pictures, the Bedrock screens they are drawn into, and the screen changes.
icon: image
---

A change here reaches players after `/uxmbedrock reload` and `/uxmbedrock pack build`.

| Key | Default | Meaning |
|---|---|---|
| `art-min-height` | `48` | A font letter at least this many Java pixels tall is a menu picture; at least 9 |
| `menus` | `{}` | Corrections per menu picture, below |
| `screens` | the shipped list | The Bedrock panels a picture is drawn into, per screen, below |
| `injection-points` | `[]` (the shipped list) | The changes to Bedrock's screen files, below |
| `hud-plugins.transparent-textures` | `["textures/ui/white_progress_bar"]` | The Bedrock textures drawn see-through, so a HUD plugin's boss bar shows only its pictures |

## menus

Each key is a picture's font letter, `"namespace:font|HEX"`, as `/uxmbedrock gui list` shows it.

```hocon
menus {
  "minecraft:default|E101" { x = -8, y = -40, screen = "chest" }
}
```

| Field | Meaning | Left out |
|---|---|---|
| `x`, `y` | The picture's top left from the panel's top left, in screen pixels | Worked out from the font |
| `width`, `height` | Its size | The picture's own |
| `layer` | 13 sits over the slot backgrounds and under the items | 13 |
| `screen` | One of the screens below | `chest` |

`/uxmbedrock gui measure` and the [preview page](../features/preview.md) write these for you.

## screens

The shipped screens: `chest`, `shulker_box`, `furnace`, `blast_furnace`, `smoker`, `anvil`, `enchanting`,
`brewing_stand`, `villager`, `crafting_table`, `inventory`, `smithing_table`, `grindstone`. A line adds a screen or
replaces one: the screen file first, then its panels.

```hocon
screens {
  chest = ["chest_screen", "small_chest_panel", "large_chest_panel"]
}
```

## injection-points

| Field | Meaning |
|---|---|
| `feature` | The pack part it belongs to: `scoreboard-art`, `java-chat`, or `ui` for everyone |
| `file` | The Bedrock screen file without `.json` |
| `element` | The element in it |
| `patch` | The properties written over it |

Empty uses the shipped list; a list here replaces it whole. The shipped points:

| Feature | File | Element | Patch |
|---|---|---|---|
| `scoreboard-art` | `scoreboards` | `scoreboard_sidebar_score` | `visible = false` |
| `scoreboard-art` | `scoreboards` | `scoreboard_sidebar_background` | `alpha = 0.0` |
| `java-chat` | `hud_screen` | `chat_panel` | `anchor_from` and `anchor_to` `bottom_left`, `size = ["320px", "100%c"]` |
