---
title: Editor windows
order: 512
description: The editor list, the property editor of one job, and the list window.
icon: pencil-ruler
---

All three need `uxmjobs.admin.editor` and are off when `editor.enabled` is false.

## The list

`menus/editor.conf`, six rows, drawn from a layout:

```hocon
layout = [
  "#########"
  "#xxxxxxx#"
  "#xxxxxxx#"
  "#xxxxxxx#"
  "#xxxxxxx#"
  "<###b##+>"
]
```

| Slot | Item | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34, 37 to 43 | A writable book per job, with its whole tooltip | Its properties |
| 45, 53 | Arrows | |
| 49 | Feather, back | Closes |
| 52 | Lime dye, new | Asks for an id |

`placeholders.tile-colour` (5) colours the tiles. Tokens: `%editor_job%`, `%tile_editor_job%`.

## Properties

`menus/editor-job.conf`: `rows` 6, `property-slots` 10 to 16, 19 to 25, 28 to 34, 37 to 43, `back-slot` 49
(`FEATHER`), `delete-slot` 53 (`BARRIER`), `filler` `GRAY_STAINED_GLASS_PANE`. The buttons are on
[In-game editor](../features/editor.md#the-properties).

## Lists

`menus/editor-list.conf`: `rows` 6, `entry-slots` 10 to 16, 19 to 25, 28 to 34, `add-slot` 53 (`LIME_DYE`), `back-slot`
45 (`FEATHER`), `entry-icon` `PAPER`, `filler` `GRAY_STAINED_GLASS_PANE`. The requirements list and the curve shape
pick are edited here.

Related: [In-game editor](../features/editor.md)
