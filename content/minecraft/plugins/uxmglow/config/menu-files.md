---
title: Menu files
order: 603
description: The keys of menus/glow.conf, the tile line, the tokens and the actions.
icon: layout-template
---

`menus/glow.conf`: the rows, the slots the sixteen colours are paged through, the material of every square, the sounds
a click plays, where the buttons sit, and which block of the language file each tooltip reads. Nothing about the window
is decided in the code.

## The window

| Key | Meaning |
|---|---|
| `title` | `@menu.title`, or your own words |
| `rows` | `5` |
| `open-actions` | A page-turn sound |
| `placeholders` | `role-free`, `role-worn`, `role-locked`, `role-glow-on`, `role-glow-off`: the title colour of each state |

## The colours list

```hocon
colours {
  slots = ["10-16", "19-25", "30", "32"]
  list {
    source = "glow:colours"
    template {
      material = "tinted:LEATHER_CHESTPLATE"
      name = " "
      lore = ["tile:%colour_role% @menu.colour.%colour_state% state"]
      decor { glow = "%colour_worn%" }
      click {
        left {
          requirements = ["glow:permitted"]
          click = ["glow:choose", "sound:BLOCK_NOTE_BLOCK_PLING 0.6 1.5"]
          deny  = ["glow:refuse", "sound:BLOCK_NOTE_BLOCK_BASS 0.6 0.9"]
        }
      }
    }
  }
}
```

A click with `requirements` runs `click` when they hold and `deny` when they do not.

## Tokens

| Token | Value |
|---|---|
| `%colour%` | The colour's id |
| `%colour_name%` | Its name |
| `%colour_state%` | `free`, `worn` or `locked` |
| `%colour_role%` | The window's `role-<state>` |
| `%colour_worn%` | `true` for the worn colour |
| `%glow_state%` | `on` or `off` |
| `%glow_role%` | The window's `role-glow-<state>` |

## Actions and conditions

| Name | Does |
|---|---|
| `glow:colours` | The list source: the sixteen colours |
| `glow:permitted` | A condition: the player holds this colour |
| `glow:choose` | Glows in this colour |
| `glow:refuse` | Says `error.colour-not-permitted` |
| `glow:clear` | Turns the glow off |

`tinted:<material>` dyes a leather item with the square's colour; another material is drawn untinted. The tile line
`tile:<colour> @<block> <facts>` draws the house tooltip from a block of the messages files, as in every UXM plugin.

Delete the file and the picker is gone: `/glow` answers `menu.missing`.

Related: [Picker](../menus/picker.md), [Messages](messages.md)
