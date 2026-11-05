---
title: text.conf
order: 602
description: Exact colours and how the private use pages are shared out.
icon: palette
---

## exact-colours

| Key | Default | Meaning |
|---|---|---|
| `charset` | `""` | The letters a colour sheet holds. Empty: printable ASCII with Turkish and the common European accents, small capitals, and the marks and signs Java's font draws. At most 256; fewer letters fit more colours on a page |
| `tolerance` | `6000` | How close two colours must be to share a sheet, as a squared distance in RGB. `0` gives every colour its own sheet until the pages are full |

## glyph-pages

Two hexadecimal digits per page: `E0` holds the letters U+E000 to U+E0FF. The three lists must not share a page; a
list that does falls back to the shipped split.

| Key | Default | Holds |
|---|---|---|
| `colours` | `E0` to `EF` | Colour sheets, two per page |
| `faces` | `F0` | One letter per face |
| `fonts` | `F1` to `F8` | The pictures of other plugins' fonts and menu backgrounds |

Every server of a network must have the same lists, since the letters are shared through the database. Changing them
after players joined draws every sheet again. A pack plugin that already uses one of these pages needs the lists
moved.
