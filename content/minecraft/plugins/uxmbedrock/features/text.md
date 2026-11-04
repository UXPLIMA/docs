---
title: Text and colours
order: 1301
description: "How text a Bedrock player reads is rewritten: colours, font, faces, pictures and click words."
icon: type
---

| Switch | What changes |
|---|---|
| `exact-colours` | A colour Bedrock does not have is drawn with letters from a sheet for that colour, in white so the client does not tint them. Gradients use the same sheets |
| `java-font` | The pack replaces Bedrock's font with Java's, so accents, small capitals and symbols look as on Java |
| `faces` | `<head:Name>` draws the player's face. A face seen for the first time shows Steve until the next build |
| `emoji` | The pictures of other plugins' fonts (emoji, icons, ranks) are drawn. A picture the pack does not hold is left out, rather than showing a box |
| `tap-words` | In item lore the word for click becomes the word for tap for a touch player and press for a controller player |
| `safe-placeholder` | `%uxmbedrock_safe_<placeholder>%` gives another placeholder in colours Bedrock draws |

## Colour sheets

A sheet holds the letters of `exact-colours.charset` in [text.conf](../config/text-conf.md): printable ASCII with the
Turkish and common European accents when the line is empty. A letter outside the set keeps its colour, which Geyser
draws in the nearest of Bedrock's sixteen. Colours closer than `exact-colours.tolerance` share one sheet, so a long
gradient uses few sheets and keeps its shape.

Chat gets faces and font pictures but not exact colours. Everything else a player reads gets all of them.

## Click words

The words come from the message files: `words.click-words` lists every way a language writes click,
`words.tap` and `words.press` are what a touch or controller player reads instead. A server in another language adds
its own words there.

## Touch players

A touch or pocket-layout player never gets menu pictures, which are laid out for the classic screen: their letters are
left out of titles. See [Menu pictures](menu-art.md).
