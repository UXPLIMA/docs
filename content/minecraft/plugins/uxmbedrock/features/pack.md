---
title: The pack
order: 1305
description: Which packs are converted, what each part becomes, the output and the build.
icon: package
---

## Sources

| Source | Found at |
|---|---|
| Nexo | `plugins/Nexo/pack/pack.zip` |
| ItemsAdder | `plugins/ItemsAdder/output/generated.zip` or `output/` |
| Oraxen | `plugins/Oraxen/pack/pack.zip` |
| CraftEngine | `plugins/CraftEngine/generated/resource_pack.zip` or the folder |
| BetterHud | `plugins/BetterHud/build.zip` or `build/` |
| MythicHUD | `plugins/MythicHUD/generated.zip` or `pack/` |
| HappyHUD | `plugins/HappyHUD/pack.zip` or `pack/` |
| CustomNameplates, CustomFishing | `plugins/<plugin>/ResourcePack` |
| LuxDialogues | `plugins/LuxDialogues/Output/LuxDialogues.zip` or the folder |
| server.properties | The pack `resource-pack` names, fetched when `sources.server-properties` is on (up to 512 MB) |
| Your own | Every zip or folder in `sources.extra` of [pack.conf](../config/pack-conf.md) |

Only the packs these plugins have built are read, never their configuration. A plugin that protects its pack is named
in `/uxmbedrock pack report` with the setting to change.

## What becomes what

| Java | Bedrock |
|---|---|
| Item models, item model definitions, old custom model data | Custom items Geyser registers, with icons; 3D models as geometry and attachables |
| Armour and elytra equipment | Attachables on the player |
| Blocks with custom states | Custom blocks Geyser registers; animated textures as flipbooks |
| Sounds | Bedrock sounds, re-encoded smaller with ffmpeg when it is installed |
| Fonts | Glyph pages and menu pictures |
| Language files | Geyser's locale overrides, so items keep their names |
| Custom heads | Heads Geyser registers by their skin |

Geyser needs the vanilla item under each custom item. It is read from the packs, learned the first time a player
holds, picks up, places or sees an item with that model, or written in [items.conf](../config/items-conf.md).
`/uxmbedrock items unknown` lists the ones still missing.

## The output

`plugins/uxmBedrock/output/` holds what the Geyser extension serves:

| File | Holds |
|---|---|
| `NN.mcpack` | The content packs: items, blocks, sounds, glyphs, then your merged Bedrock packs |
| `feature-<part>.mcpack` | One pack per part a player may switch off, and `feature-ui.mcpack` for everyone |
| `animated.mcpack`, `animated-light.mcpack` | Animated content, full and light |
| `items.json`, `blocks.json`, `locales/` | Geyser's mappings and names |
| `choices.json`, `skulls.json` | Each player's choices; the heads |
| `result.json`, `screen.json`, `report.txt` | The index, the menu pictures, the report |

A pack's version moves with its content, so a client downloads only a pack that changed. A build whose sources did
not change comes from `cache/` at once.

## When it builds

| When | Builds |
|---|---|
| Start | Always, from the cache when nothing changed |
| A source changes | When `auto-convert` is on, checked every `check-every-seconds` |
| `/uxmbedrock pack build` | Now |
| A measure or a preview save | With the new place |

A pack switched off under `modules` is taken out of the build, so no player gets it.
