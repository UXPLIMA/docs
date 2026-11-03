---
title: Editor windows
order: 413
description: The editor list, the property editor of one type, and the list window.
icon: pencil-ruler
---

All three need `uxmminions.admin.editor` and are off when `editor.enabled` is false.

## The list

`menus/editor.conf`, six rows from a layout (`#` panes, `x` types, `<` and `>` arrows at 45 and 53, `b` back at 49,
`+` new at 52). Each type is a tile with its whole tooltip; a click opens its properties; the lime dye asks for an id.
Tokens: `%editor_type%`, `%tile_editor_type%`.

## Properties

`menus/editor-type.conf`: `rows` 6, `property-slots` 10 to 16, 19 to 25, 28 to 34, 37 to 43, `back-slot` 49
(`FEATHER`), `delete-slot` 53 (`BARRIER`), `filler` `GRAY_STAINED_GLASS_PANE`.

## Lists

`menus/editor-list.conf`: `rows` 6, `entry-slots` 10 to 16, 19 to 25, 28 to 34, `add-slot` 53 (`LIME_DYE`),
`back-slot` 45 (`FEATHER`), `entry-icon` `PAPER`, `filler` `GRAY_STAINED_GLASS_PANE`.

Related: [In-game editor](../features/editor.md)
