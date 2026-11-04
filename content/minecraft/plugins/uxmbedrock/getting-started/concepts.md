---
title: Concepts
order: 203
description: Packets, the pack and registration; glyph pages; feature switches and player choices.
icon: lightbulb
---

## Three parts

| Part | Where it runs | What it does |
|---|---|---|
| Packets | The Paper server | The packets that carry text, scoreboards, the player list, titles, boss bars, displays and menus are rewritten for each Bedrock player before Geyser translates them |
| The pack | The Paper server builds it, the Geyser extension serves it | One Bedrock pack made from the server's resource packs, with the glyph pages, the JSON UI changes and the converted content |
| Registration | The Geyser extension | Custom items, blocks and heads are given to Geyser when it starts |

A new colour, face or font picture is in the pack from the player's next join. A new custom item, block or head needs a
Geyser restart; the console says when one is needed.

## Glyph pages

Bedrock draws sixteen colours. For any other colour the pack holds a sheet of letters drawn in that colour, and a
Bedrock player's text is written with those letters in white, so the client does not tint them. Faces and the
pictures of other plugins' fonts get a letter each the same way. The letters live in Unicode's private use pages,
which [text.conf](../config/text-conf.md) shares out:

| Pages | Hold |
|---|---|
| `E0` to `EF` | Colour sheets, two of up to 128 letters per page |
| `F0` | Faces |
| `F1` to `F8` | Font pictures: emoji, icons, ranks, menu backgrounds |

The slots live in the database, so every server of a network draws a colour with the same letters. A colour seen for
the first time takes a slot and is drawn exactly from the next build; until then it shows in the nearest colour that
already has a sheet, or the nearest of Bedrock's sixteen.

## Switches and choices

| Layer | Set in | Decides |
|---|---|---|
| Module switch | `modules` in [config.conf](../config/config-conf.md) | Whether a feature runs at all on this server |
| Player choice | `player-choices.allowed` in config.conf, then `/bedrock` | Which features a Bedrock player may switch off for themselves |
| Menu rules | [menus.conf](../config/menus-conf.md) | Which menus of other plugins open as forms |

A player's choices are kept in the database and in `output/choices.json`, which the Geyser extension reads at their
next join to leave out the parts of the pack they switched off.
