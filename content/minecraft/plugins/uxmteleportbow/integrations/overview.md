---
title: Overview
order: 701
description: Every plugin uxmTeleportBow detects, and what it turns on.
icon: book-open
---

Every integration is optional and none is switched on by hand. uxmTeleportBow looks for each plugin when it needs
it; with the plugin absent, no class of it is ever loaded.

| Plugin | Turns on | Page |
|---|---|---|
| WorldGuard | Region names for `filters.regions` | [WorldGuard and Towny](regions.md) |
| Towny | Town names for `filters.regions` | [WorldGuard and Towny](regions.md) |
| PlaceholderAPI | The `%uxmteleportbow_...%` placeholders, and other plugins' placeholders inside an effect line's condition | [PlaceholderAPI](placeholderapi.md) |
| Vault | `[take-money]` and `[give-money]` in an effect line, through the economy Vault hands out | [Effects](../bow/effects.md#writing-a-line) |
| mcMMO | `%mcmmo_level_<skill>%` in an effect line's condition, read from mcMMO without PlaceholderAPI | [Effects](../bow/effects.md#when-and-how-often) |
| uxmLang | One language choice for every UXM plugin | [Languages](../config/languages.md#where-a-choice-is-kept) |

## With other UXM plugins

| Plugin | Together |
|---|---|
| uxmJoinItems | Hands out the lobby's other hotbar items; give each plugin its own slot |
| uxmPlayerHider | Its hotbar item sits beside the bow; give each its own slot |
| uxmLang | The language a player chose is read here |
| uxmTheme | Not a plugin: the shared `plugins/uxmTheme/theme.conf` every UXM plugin reads |

Related: [WorldGuard and Towny](regions.md), [PlaceholderAPI](placeholderapi.md)
