---
title: Overview
order: 901
description: Every plugin uxmBedrock hooks into and what each turns on.
icon: puzzle
---

| Plugin | Needed | What it turns on |
|---|---|---|
| Floodgate | Yes, on every Paper server | Telling Bedrock players apart, their device, input and layout, forms |
| Geyser | Yes, on the server, the proxy or standalone | The extension: packs per player, items, blocks, heads |
| PlaceholderAPI | No | The [placeholders](../placeholders/reference.md) |
| Nexo, ItemsAdder, Oraxen, CraftEngine | No | Their built packs are converted: items, furniture, blocks, armour, fonts, menu backgrounds |
| BetterHud, MythicHUD, HappyHUD | No | Their packs are converted and their HUD drawn |
| CustomNameplates, CustomFishing, LuxDialogues | No | Their packs are converted |
| uxmLang | No | One language choice for every UXM plugin |
| Velocity | No | One pack for the network and proxy command completion, with uxmBedrock-Velocity |
| ffmpeg | No, a program on the machine | Smaller sounds |

## Plugins that protect their pack

Some pack plugins can protect the pack they write so other tools cannot read it. uxmBedrock reads only the built pack;
when a plugin's protection keeps it from reading, `/uxmbedrock pack report` names the plugin, the file and the
setting to change.

## Plugins built on uxmLib

Menus of UXM plugins already open as Bedrock forms of their own and are not turned into forms again. The choice a
player makes with `/menus` is read by those menus too: a player who chose chests gets chests everywhere.
