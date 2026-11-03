---
title: Editors
order: 408
description: The list of objects, the properties of one, and the window a list of lines is edited in.
icon: pencil
---

## The list

`/chateditor channels`, `tags`, `announcements` or `announcement`. Six rows; the objects sit in slots 10 to 16, 19 to
25, 28 to 34 and 37 to 43. Arrows at 48 and 50.

| Slot | Click |
|---|---|
| An object | Opens its properties |
| 53, a lime dye | Asks for an id and makes a new object under it |

## Properties

`menus/editor-properties.conf`. One button per field in slots 10 to 16, 19 to 25 and 28 to 34; back at 49 (a
feather), delete at 53 (a barrier).

## Lines

`menus/editor-list.conf`. The lines of one list field, such as a tag's frames, a price's conditions or a bundle's
announcements, in slots 10 to 16, 19 to 25, 28 to 34 and 37 to 43, as paper; add at 53 (a lime dye), back at 49 (a
feather).

These two files take keys rather than items: `rows`, `property-slots` or `entry-slots`, `add-slot`, `back-slot`,
`delete-slot`, and the materials `entry-icon`, `add-icon`, `back-icon`, `delete-icon` and `filler`.

Related: [In-game editors](../features/editors.md)
