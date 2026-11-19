---
title: World
order: 1307
description: Furniture and decorations of item plugins, and custom heads.
icon: box
---

| Switch | What changes |
|---|---|
| `displays` | Item and block displays (the furniture and decorations of item plugins and the models of ModelEngine and BetterModel), which Bedrock has no entity for, are drawn with their 3D models, turned, moved and scaled as Java draws them. An item frame holding an item with a 3D look shows it in 3D as Java does, where Bedrock draws a flat icon. Before the Geyser extension answers, a display shows as an invisible stand wearing the item |
| `skulls` | Every custom head a player meets is registered with Geyser by its skin, so it shows the skin rather than Steve |
| `biomes` | The biomes of data packs, which Geyser draws as an ocean, are drawn as the game's biome that looks most like each: its water, grass and leaf colours, rain and snow. Geyser reads the list at its next start |

New heads reach Geyser at its next restart; `/uxmbedrock skulls` counts the ones known. Heads are shared through the
database, so every server of a network registers the heads any of them saw.
