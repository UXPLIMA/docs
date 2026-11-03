---
title: Overview
order: 901
description: Every plugin uxmEasterEggs detects, and what it turns on.
icon: book-open
---

Every integration is optional and none is switched on by hand. uxmEasterEggs looks for each plugin at start; with
the plugin absent, no class of it is ever loaded.

| Plugin | Turns on | Page |
|---|---|---|
| PlaceholderAPI | The `%uxmeastereggs_...%` placeholders, and other plugins' placeholders in a condition | [PlaceholderAPI](placeholderapi.md) |
| Vault | Rewards paying `vault` | [Economies](economies.md) |
| VaultUnlocked | Rewards paying `vault-unlocked` | [Economies](economies.md) |
| PlayerPoints | Rewards paying `playerpoints` | [Economies](economies.md) |
| EcoBits | Rewards paying `ecobits:<currency>` | [Economies](economies.md) |
| Treasury | Rewards paying `treasury` | [Economies](economies.md) |
| mcMMO | `%mcmmo_level_<skill>%` in a condition, read from mcMMO without PlaceholderAPI | [Moments](../hunting/moments.md#when-and-how-often) |
| Geyser, Floodgate | Bedrock forms instead of chest windows | [Menus](../menus/overview.md#bedrock-players) |
| uxmLang | One language choice for every UXM plugin | [Languages](../config/languages.md#where-a-choice-is-kept) |
| A hologram plugin | The board on a hologram, through PlaceholderAPI | [Placeholders](../placeholders/reference.md#a-hologram) |

## With other UXM plugins

| Plugin | Together |
|---|---|
| uxmEssentials | Its holograms draw the board with heads |
| uxmJoinItems | A hotbar item that runs `/eggs` |
| uxmLang | The language a player chose is read here |
| uxmTheme | Not a plugin: the shared `plugins/uxmTheme/theme.conf` every UXM plugin reads |

Related: [Economies](economies.md), [PlaceholderAPI](placeholderapi.md)
