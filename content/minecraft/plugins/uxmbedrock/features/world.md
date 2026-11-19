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
| `biomes` | The biomes of data packs, which Geyser draws as an ocean, are drawn in their own colours: grass, leaves, dry leaves, water, sky and fog, with grass and leaves worked out from Java's colour maps as Java does. Each takes one of the 27 Bedrock biomes of old worlds that no Java biome uses, one where it snows for a snowy biome and one where nothing falls for a dry one, so rain and snow fall as on Java. Past 27, a biome is drawn as the game's biome that looks most like it. A biome changed while a player is near (`/fillbiome`, a season plugin) reaches a Bedrock player at once, as on Java. Players see a new data pack from their next join |

New heads reach Geyser at its next restart; `/uxmbedrock skulls` counts the ones known. Heads are shared through the
database, so every server of a network registers the heads any of them saw.
