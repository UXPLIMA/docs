---
title: What Bedrock cannot do
order: 1311
description: What no plugin can change on Bedrock, and what uxmBedrock does instead.
icon: circle-alert
---

| Java has | On Bedrock | What uxmBedrock does |
|---|---|---|
| The player list header and footer | Bedrock has no tab key and its player list takes no header or footer | The names, colours and faces are drawn; header and footer are not |
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
