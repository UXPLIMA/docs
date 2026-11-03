---
title: Overview
order: 901
description: Every plugin uxmKotl detects, and what it turns on.
icon: book-open
---

Every integration is optional and none is switched on by hand. uxmKotl looks for each plugin at start; with the
plugin absent, no class of it is ever loaded.

| Plugin | Turns on | Page |
|---|---|---|
| PlaceholderAPI | The `%uxmkotl_...%` placeholders, and other plugins' placeholders inside a moment's condition | [PlaceholderAPI](placeholderapi.md) |
| Vault | Rewards paying `vault` | [Economies](economies.md) |
| VaultUnlocked | Rewards paying `vault-unlocked` | [Economies](economies.md) |
| PlayerPoints | Rewards paying `playerpoints` | [Economies](economies.md) |
| EcoBits | Rewards paying `ecobits:<currency>` | [Economies](economies.md) |
| Treasury | Rewards paying `treasury` | [Economies](economies.md) |
| mcMMO | `%mcmmo_level_<skill>%` in a moment's condition, read from mcMMO without PlaceholderAPI | [Moments](../arenas/moments.md#when-and-how-often) |
| Geyser, Floodgate | Bedrock forms instead of chest windows | [Menus](../menus/overview.md#bedrock-players) |
| uxmLang | One language choice for every UXM plugin | [Languages](../config/languages.md#where-a-choice-is-kept) |
| A hologram plugin | The king and the boards on a hologram, through PlaceholderAPI | [Placeholders](../placeholders/reference.md#a-hologram) |

## With other UXM plugins

| Plugin | Together |
|---|---|
| uxmEssentials | Its holograms draw the king and the boards with heads; its NPCs can run `kotl join <arena>` as the player |
| uxmJoinItems | A hotbar item that runs `/kotl join <arena>` |
| uxmLang | The language a player chose is read here |
| uxmTheme | Not a plugin: the shared `plugins/uxmTheme/theme.conf` every UXM plugin reads |

Related: [Economies](economies.md), [PlaceholderAPI](placeholderapi.md)
