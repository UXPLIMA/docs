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
| TCPShield | No | Checked: Bedrock players can join only with its Geyser support on |

## Tested versions

Each plugin was checked in game at this version, a Java and a Bedrock client side by side. Another version works
as long as the plugin keeps the pack it builds and its API alike; `/uxmbedrock pack report` names what a build left out.

| Plugin | Version |
|---|---|
| Geyser | 2.12.1 |
| Floodgate | 2.2.5 |
| Nexo | 1.29.1 |
| ItemsAdder | 5.0.0-beta-8 |
| Oraxen | 1.220.1 |
| CraftEngine | 26.9.2 |
| ModelEngine | R4.2.0 |
| BetterModel | 3.6.0 |
| BetterHud | 2.2.0 |
| CustomNameplates | 3.0.45 |
| CustomFishing | 2.3.28 |
| LuxDialogues | 4.1.5 |

## Plugins that protect their pack

Some pack plugins can protect the pack they write so other tools cannot read it. uxmBedrock reads only the built pack;
when a plugin's protection keeps it from reading, `/uxmbedrock pack report` names the plugin, the file and the
setting to change.

## TCPShield

TCPShield lets connections in only through its own network, and refuses Geyser's own connection unless its Geyser
support is on; then no Bedrock player can join at all. When TCPShield runs on the server, uxmBedrock reads its setting
at start: with `enable-geyser-support: false` in `plugins/TCPShield/config.yml`, the console says so and
`/uxmbedrock doctor` repeats it. Set it to `true` (TCPShield needs Floodgate for it) and restart.

## Plugins built on uxmLib

Menus of UXM plugins already open as Bedrock forms of their own and are not turned into forms again. The choice a
player makes with `/menus` is read by those menus too: a player who chose chests gets chests everywhere.
