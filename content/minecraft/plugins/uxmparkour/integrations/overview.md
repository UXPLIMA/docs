---
title: Overview
order: 801
description: Every plugin uxmParkour detects, and what it turns on.
icon: book-open
---

Every integration is optional and none is switched on by hand. uxmParkour looks for each plugin at start; with the
plugin absent, no class of it is ever loaded.

| Plugin | Turns on | Page |
|---|---|---|
| PlaceholderAPI | The `%uxmparkour_...%` placeholders, and other plugins' placeholders inside this plugin's conditions | [PlaceholderAPI](placeholderapi.md) |
| Vault | Milestones paying `vault` | [Economies](economies.md) |
| VaultUnlocked | Milestones paying `vault-unlocked` | [Economies](economies.md) |
| PlayerPoints | Milestones paying `playerpoints` | [Economies](economies.md) |
| EcoBits | Milestones paying `ecobits:<currency>` | [Economies](economies.md) |
| Treasury | Milestones paying `treasury` | [Economies](economies.md) |
| mcMMO | `%mcmmo_level_<skill>%` in a moment's condition, read from mcMMO without PlaceholderAPI | [Moments](../arenas/moments.md#when-and-how-often) |
| Geyser, Floodgate | Bedrock forms instead of chest windows | [Menus](../menus/overview.md#bedrock-players) |
| uxmLang | One language choice for every UXM plugin | [Languages](../config/languages.md#where-a-choice-is-kept) |
| A hologram plugin | The boards on a hologram, through PlaceholderAPI | [Placeholders](../placeholders/reference.md#a-hologram) |

## With other UXM plugins

| Plugin | Together |
|---|---|
| uxmEssentials | Its holograms draw the boards with heads; its NPCs can run `parkour join <arena>` as the player |
| uxmJoinItems | A hotbar item that runs `/parkour` |
| uxmLang | The language a player chose is read here |
| uxmTheme | Not a plugin: the shared `plugins/uxmTheme/theme.conf` every UXM plugin reads |

Related: [Economies](economies.md), [PlaceholderAPI](placeholderapi.md)
