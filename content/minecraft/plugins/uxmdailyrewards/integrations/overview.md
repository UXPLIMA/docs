---
title: Overview
order: 901
description: Every plugin uxmDailyRewards detects, and what it turns on.
icon: book-open
---

Every integration is optional and none is switched on by hand. uxmDailyRewards looks for each plugin at start; with
the plugin absent, no class of it is ever loaded.

| Plugin | Turns on | Page |
|---|---|---|
| LuckPerms | Ranks from the player's groups | [Ranks](../rewards/ranks.md) |
| Vault | `[give-money] vault` | [Economies](economies.md) |
| VaultUnlocked | `[give-money] vault-unlocked` | [Economies](economies.md) |
| PlayerPoints | `[give-money] playerpoints` | [Economies](economies.md) |
| EcoBits | `[give-money] ecobits:<currency>` | [Economies](economies.md) |
| Treasury | `[give-money] treasury` | [Economies](economies.md) |
| PlaceholderAPI | The `%uxmdailyrewards_...%` placeholders, and other plugins' placeholders inside a line's condition | [PlaceholderAPI](placeholderapi.md) |
| mcMMO | `%mcmmo_level_<skill>%` in a line's condition, read from mcMMO without PlaceholderAPI | [Day files](../rewards/day-files.md#writing-a-line) |
| Geyser, Floodgate | The calendar as a Bedrock form | [Menus](../menus/overview.md#bedrock-players) |
| uxmLang | One language choice for every UXM plugin | [Languages](../config/languages.md#where-a-choice-is-kept) |

## With other UXM plugins

| Plugin | Together |
|---|---|
| uxmJoinItems | A hotbar item that runs `/dailyrewards` |
| uxmEssentials | An NPC or a hologram that opens the calendar, and its economy through Vault |
| uxmCrates | A day that gives a crate key with a `[console]` line, named in the window with `menu.day.rewards.<day>` |
| uxmLang | The language a player chose is read here |
| uxmTheme | Not a plugin: the shared `plugins/uxmTheme/theme.conf` every UXM plugin reads |

Related: [Economies](economies.md), [PlaceholderAPI](placeholderapi.md)
