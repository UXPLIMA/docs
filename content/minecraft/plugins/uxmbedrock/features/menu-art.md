---
title: Menu pictures
order: 1304
description: Custom menu backgrounds of item plugins drawn over the Bedrock screen.
icon: image
---

Item plugins draw a custom menu background with a tall letter of their font in the menu's title. Bedrock draws titles
its own way, so the pack draws that picture over the Bedrock screen instead, whenever a title holds the letter, and
the letter itself shows nothing.

| Setting | In | Does |
|---|---|---|
| `menu-art` | `modules` in config.conf | Turns menu pictures on |
| `art-min-height` | gui.conf | A font letter at least this many Java pixels tall is a menu picture, not an emoji. 48 |
| `menus` | gui.conf | Corrections per picture: place, size, layer, screen |
| `screens` | gui.conf | The Bedrock panels a picture is drawn into, per screen |

## Placing a picture

Where a picture sits is worked out from the font: its left edge at the panel's left edge, its top from the font's
ascent. When it sits wrong:

| Way | How |
|---|---|
| Measure | `/uxmbedrock gui measure`, then open the menu as a Java player. The place is read from the title's spaces and written to gui.conf, and the pack is built again |
| Preview | Move and size it in the [preview page](preview.md), then save |
| By hand | Write `x`, `y`, `width`, `height`, `layer` or `screen` under the picture's key in gui.conf, then `/uxmbedrock reload` and `/uxmbedrock pack build` |

`/uxmbedrock gui list` lists every picture's key and place. Touch and pocket-layout players never get pictures; see
`touch` in [menus.conf](../config/menus-conf.md).
