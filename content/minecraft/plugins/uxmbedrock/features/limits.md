---
title: What Bedrock cannot do
order: 1311
description: What no plugin can change on Bedrock, and what uxmBedrock does instead.
icon: circle-alert
---

| Java has | On Bedrock | What uxmBedrock does |
|---|---|---|
| A line through or under text | Bedrock draws no lines and leaves a unit after every letter of the pack | Lines are drawn into the letters; a run of spaces is drawn four to a letter, so a rule shows a small gap every 16 units |
| Symbols drawn in half units (unifont: the coin, the four pointed star, most arrows) | A letter of the pack is never finer than one unit | Drawn from the pack's font at Java's size and detail, in the nearest of Bedrock's sixteen colours |
| Space letters that move text left | Bedrock text cannot move left | HUD pictures keep their order without the left moves |
| Custom biome colours | Geyser maps every custom biome onto a vanilla Bedrock biome | Not converted |
| Riding and seat camera from jump and sneak | Needs Geyser's input internals | Not changed |
| ModelEngine, BetterModel and cosmetic models | No Geyser API to draw them | Not converted |
| Furniture as blocks | A Geyser custom block needs a Java block state of its own | Furniture shows as a display stand |
| Menu pictures on touch screens | The pocket layout has other panels | Touch players get the plain chest or a form |
| Score numbers hidden by Java's number format | Bedrock always draws numbers | The pack hides the score column |
| Animated textures on weak devices | Every frame is downloaded | A light pack keeps every second frame; `none` keeps the first frame |

What a Bedrock player sees of glyph sheets, menu pictures and screen changes depends on Bedrock's own screen files.
When a Bedrock update moves an element, repoint the injection point in [gui.conf](../config/gui-conf.md).
