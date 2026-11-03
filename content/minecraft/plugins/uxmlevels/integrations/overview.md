---
title: Overview
order: 901
description: Every plugin uxmLevels detects, and what it turns on.
icon: book-open
---

Every integration is optional and none is switched on by hand. uxmLevels looks for each plugin at start.

| Plugin | Turns on | Page |
|---|---|---|
| Vault, VaultUnlocked, PlayerPoints, EcoBits, Treasury | A prestige money cost, and `[give-money]` in a reward | [Economies](economies.md) |
| PlaceholderAPI | The `%uxmlevels_...%` placeholders, and other plugins' placeholders in a reward line's condition | [PlaceholderAPI](placeholderapi.md) |
| mcMMO | `%mcmmo_level_<skill>%` in a reward line's condition, without PlaceholderAPI | [Rewards](../tracks/rewards.md#writing-a-line) |
| Geyser, Floodgate | The windows as Bedrock forms | [Menus](../menus/overview.md#bedrock-players) |
| uxmLang | One language choice for every UXM plugin | [Languages](../config/languages.md#where-a-choice-is-kept) |
| A permission plugin | Multipliers by group, and keeping a group off a track | [Permissions](../permissions/reference.md) |

## With other UXM plugins

| Plugin | Together |
|---|---|
| uxmChat | A level or a badge in a chat format through PlaceholderAPI |
| uxmEssentials | Its holograms draw the boards with heads |
| uxmSkills | A separate system: skills level by doing one thing, tracks by any event you write. Both can run |
| uxmLang | The language a player chose is read here |
| uxmTheme | Not a plugin: the shared `plugins/uxmTheme/theme.conf` every UXM plugin reads |

Related: [Economies](economies.md), [PlaceholderAPI](placeholderapi.md)
