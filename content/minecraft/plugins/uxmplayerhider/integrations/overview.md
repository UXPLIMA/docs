---
title: Overview
order: 801
description: Every plugin uxmPlayerHider detects, and what it turns on.
icon: book-open
---

Every integration is optional and none is switched on by hand. uxmPlayerHider looks for each plugin at start; with
the plugin absent, no class of it is ever loaded.

| Plugin | Turns on | Page |
|---|---|---|
| PlaceholderAPI | The `%uxmplayerhider_...%` placeholders, and other plugins' placeholders inside an effect line's condition | [PlaceholderAPI](placeholderapi.md) |
| Vault | `[take-money]` and `[give-money]` in an effect line, through the economy Vault hands out | [Effects](../hiding/effects.md#writing-a-line) |
| mcMMO | `%mcmmo_level_<skill>%` in an effect line's condition, read from mcMMO without PlaceholderAPI | [Effects](../hiding/effects.md#when-and-how-often) |
| uxmLang | One language choice for every UXM plugin | [Languages](../config/languages.md#where-a-choice-is-kept) |
| A permission plugin | Who is important, and who skips the cooldown | [Permissions](../permissions/reference.md) |

## With other UXM plugins

| Plugin | Together |
|---|---|
| uxmJoinItems | Hands out the lobby's other hotbar items; give each plugin its own slot |
| uxmTeleportBow | Its bow sits in hotbar slot 2 by default, the tool in slot 8 |
| uxmLang | The language a player chose is read here |
| uxmTheme | Not a plugin: the shared `plugins/uxmTheme/theme.conf` every UXM plugin reads |

Related: [PlaceholderAPI](placeholderapi.md)
