---
title: World
order: 1307
description: Furniture and decorations of item plugins, and custom heads.
icon: box
---

| Switch | What changes |
|---|---|
| `displays` | Item and block displays (the furniture and decorations of item plugins), which Bedrock has no entity for, show as an invisible stand wearing the item, at the display's scale |
| `skulls` | Every custom head a player meets is registered with Geyser by its skin, so it shows the skin rather than Steve |

New heads reach Geyser at its next restart; `/uxmbedrock skulls` counts the ones known. Heads are shared through the
database, so every server of a network registers the heads any of them saw.
